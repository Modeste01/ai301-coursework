# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Plan context (scope, files, test plan, deviation notes) read against Candidate PR diff and description claims | The diff implements the changes authorized in the plan without unannounced additions (unrequested refactors, new flags, extra configs, or unrelated file changes). Planned changes omitted from the diff are acceptable only if explicitly documented as an honest shortfall or deferral in both plan deviation notes and the PR description. The description does not claim fixes or changes absent from the diff. | required |
| decisive-test-evidence | Candidate PR test evidence section read against the plan's test plan and the original bug's reproduction steps | The test evidence shows observable before/after verification on the specific failure path demonstrated by the bug reproduction (not merely an unchanged control path, and not bare assertions like "tested locally" or "cargo test passes" without showing the repro). If the plan specified multiple repro cases or repo checks, they are evidenced or accounted for. | required |
| clean-diff-quality | Candidate PR diff and commit messages read for code hygiene and focus | The diff contains only the intended change and is free of review-blocking debris: no commented-out code experiments, no debug print/log statements (e.g. eprintln, print), no dead or unused functions/probes, and no extraneous whitespace, formatting, or import churn unrelated to the fix. | required |
| standards-and-disclosure | Candidate PR title and description read against Repo facts (PR template asks, contributing docs, and AI disclosure policy) | The PR description substantively completes all required template sections and checklists (including referencing the target issue, e.g. Closes #), and explicitly includes any AI-use disclosure required by the repository's stated contribution policy. | required |

## Verdict rule

Accept if every required check passes; preferred checks never change the verdict; unclear counts as fail on required checks.
