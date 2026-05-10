# Issues and Constraints

This file tracks known issues, debt, tradeoffs, and unresolved questions.

## Rules

- Move an item out when it becomes actionable work
- Keep durable findings here, not transient execution state

### Publish Automation Boundary

- Type: design tradeoff
- Details: The user wants a `公開エージェント`, but this repository is currently a Zenn content repo without configured external publishing credentials. The current design limits that role to publish-readiness checks and `published` toggle guidance.
- Impact: Avoids overcommitting to WordPress or other CMS automation before the real target and auth model are known.
- Mitigation: Revisit when a concrete external publish target is selected.
- Related: `docs/requirements.md`, `docs/agent-team/prompts/generated/publish-specialist.md`

### Analytics Data Source Not Chosen

- Type: unspecified requirement
- Details: A `分析エージェント` is defined, but no canonical metrics source has been chosen yet.
- Impact: The analytics workflow can only operate on data the user explicitly provides for now.
- Mitigation: Choose a source such as Zenn stats, Search Console, or GA before implementing automation.
- Related: `docs/requirements.md`, `docs/agent-team/prompts/generated/analytics-specialist.md`
