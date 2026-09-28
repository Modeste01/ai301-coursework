# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-active` | Repo facts: archived status, last push date, and last 5 default-branch commit dates | archived is "no" AND at least one default-branch commit or push occurred within 180 days of the capture date | required |
| `unclaimed` | Repo facts: assignees and linked PRs, plus the comment thread | assignees is "none", linked PRs has no open pull requests, and the thread contains no active, unabandoned claim from another contributor within 30 days | required |
| `bounded-scope` | Issue body: problem description, proposed changes, and task scope | The issue modifies 1 to 2 identified files or docs pages, contains no sub-task checklists or tracking tables, and has no unresolved architectural debate in the comments | required |
| `ai-policy-permitted` | Repo facts: contribution policy (CONTRIBUTING.md or dedicated AI policy file) | The repository policy does not explicitly prohibit AI-assisted or AI-generated contributions (silence, disclosure rules, or human-review requirements pass) | required |
| `maintainer-responsive` | Repo facts: maintainer first-response sample, or maintainer comments in the issue thread | At least one sampled issue shows maintainer response within 30 days, or a maintainer participated in this issue's thread | preferred |
| `clear-reproduction` | Issue body: reproduction steps, stack trace, or covering test reference | The issue provides explicit reproduction steps, an error log/traceback, or names a specific target file or test | preferred |

## Verdict rule

Accept if every required check (`repo-active`, `unclaimed`, `bounded-scope`, `ai-policy-permitted`) passes. If any required check fails or is unclear, reject. Preferred checks (`maintainer-responsive`, `clear-reproduction`) never change the verdict; they serve only to rank accepted issues.
