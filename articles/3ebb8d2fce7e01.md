---
title: "SSM AutomationとSecrets Managerの相性が悪かった話"
emoji: "🔐"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "SSM", "SecretsManager", "Windows", "EC2"]
published: false
---

## はじめに

Windowsサーバーのローカルユーザー作成を自動化したいと思い、SSM Automationで組もうとしたところ、Secrets Managerとの相性で詰まってしまいました。

結論として「スッキリした構成」は作れず、最終的にはSSM Session Managerで1台ずつ手作業、という結末になったので、同じところで悩む人のために記録を残しておきます。

---

## やりたかったこと

- EC2（Windows Server）のローカルユーザーを**自動で作成**したい
- ユーザー名・パスワードは**Secrets Managerに保存**しておき、Automationから参照する
- 対象インスタンスが増えても、同じ仕組みで展開できるようにしたい

イメージとしてはこんな流れです。

```
SSM Automation → Secrets Manager から値取得 → EC2に対して net user コマンド実行
```

---

## なぜEC2にSecretsの権限を渡したくなかったか

最初に思いついたのは「EC2のIAMロールに`secretsmanager:GetSecretValue`を付与する」構成です。

しかし、これだとインスタンスにログインできる人は、誰でも以下のようにシークレットを取得できてしまいます。

```powershell
aws secretsmanager get-secret-value --secret-id my-local-user-secret
```

ローカルユーザー作成用のパスワードが、インスタンス内から自由に取れてしまうのは避けたいところ。なので、**EC2にSecretsの権限は渡さない**方針にしました。

---

## Automationに権限を持たせてみたら…

次に考えたのが、SSM Automationの実行ロールにSecrets取得権限を付与し、Automation内でシークレットを解決してからEC2に渡す、という構成です。

擬似的にはこんなイメージ。

```yaml
mainSteps:
  - name: getSecret
    action: aws:executeAwsApi
    inputs:
      Service: secretsmanager
      Api: GetSecretValue
      SecretId: my-local-user-secret
  - name: createUser
    action: aws:runCommand
    inputs:
      DocumentName: AWS-RunPowerShellScript
      Parameters:
        commands:
          - net user {{ getSecret.username }} {{ getSecret.password }} /add
```

「これならEC2には権限いらないし完璧では？」と思ったのですが、**実行結果の画面にシークレットの値がそのまま表示されてしまう**という問題に遭遇しました。

`executeAwsApi`の出力や、次ステップに渡したパラメータが**AutomationのExecution履歴にログとして残ってしまう**のです。これだとCloudTrailやAutomationの実行履歴を見られる人に、パスワードが丸見えになってしまいます。

本末転倒です。

---

## 対処法として考えたこと

### 案1: EC2からAssumeRoleでシークレットを取得

EC2に`sts:AssumeRole`の権限だけ付与して、必要なときだけ一時的にSecrets取得用のロールを引き受ける構成。

これなら、

- EC2のインスタンスプロファイル自体にはSecrets権限を持たせない
- AssumeRoleのタイミング・範囲を絞れる

というメリットはあるのですが、

- インスタンス内からAssumeRoleコマンドを叩けば結局シークレットは取れる
- Automationで組んだ他の仕組みと比べて、**この処理だけ例外的な構成**になる

という点で、個人的にはあまり採用したくありませんでした。仕組みの統一感が崩れると、後から運用する人が混乱しやすくなります。

### 案2: Automation内でマスキングする

SSM Automationのパラメータには`NoEcho`のような仕組みが一部ありますが、`executeAwsApi`で取得したシークレットを後続ステップに渡すと、**結局どこかで平文として扱われてしまう**ため、完全に隠しきるのは難しい印象でした。

---

## 結局どうしたか

色々考えた結果、**SSM Session Managerで1台ずつログインしてローカルユーザーを作成する**という、自動化を諦めた運用に落ち着きました。

- インスタンス台数がそこまで多くない
- ユーザー作成は頻繁に発生する作業ではない
- 例外的な構成を増やすより、素直に手作業のほうがリスクが低い

という判断です。

---

## 学び

- **Secrets Manager × SSM Automationは、値を「隠したまま」使うのが難しい**
  - Automationの実行履歴にパラメータが残ってしまう以上、機密情報をパラメータ経由で引き回す設計は慎重になる必要がある
- **「EC2に権限を持たせたくない」を突き詰めると、Automation側に寄せたくなるが、そこにも落とし穴がある**
- **自動化のために例外的なIAM構成を増やすくらいなら、手作業のほうが健全なケースもある**

「全部自動化したい」という気持ちはありますが、**機密情報を扱う箇所は手作業に残したほうが安全**ということを、今回の件で実感しました。

---

## おわりに

SSM AutomationとSecrets Managerを組み合わせた事例はあまり多くなく、特に「AutomationからEC2にシークレットを安全に渡す」方法については、公式ドキュメントでも明確な解が示されていない印象です。

もし**シークレットをうまく使って、自動的に配布する仕組み**をご存じの方がいれば、ぜひ教えていただけると嬉しいです。
