# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | The Environment section of the candidate repro report | Names OS, relevant runtime or dependency versions, and the repository commit or release tested; if testing a version different from the issue, acknowledges the version delta | required |
| `steps-followable` | The reproduction steps in the candidate repro report | Commands and inputs are fully specified and standalone, so a stranger can re-run them without unshared private configurations, missing drivers, or unstated external dependencies | required |
| `artifact-faithful` | Output logs, console transcripts, or stack traces in the repro report compared against the issue description | Contains concrete execution artifacts (command output, error traceback, or measurement) showing the actual behavior of the system under test; does not merely show that the software runs or assert vibes without artifacts | required |
| `behavior-matches` | The error symptom, exit code, or behavior in the artifact read against the issue's target bug | The artifact demonstrates the specific behavior, error type, or failure mechanism reported in the issue (or documents an honest cannot-reproduce with specific differing environmental parameters); does not trigger an unrelated syntax or compile error and claim it reproduces the bug | required |
| `claim-modest-and-specific` | The candidate claim comment read against the issue | The claim states a concrete intent to investigate or reproduce this specific issue without promising a guaranteed fix or deadline | required |
| `repo-conventions` | The repo-facts contribution policy and AI policy lines read against both comments | If the repository's stated policy requires disclosing AI assistance, the comment explicitly discloses AI usage; otherwise respects repo templates and communication rules | required |

## Verdict rule

accept if every required check (`environment-recorded`, `steps-followable`, `artifact-faithful`, `behavior-matches`, `claim-modest-and-specific`, `repo-conventions`) passes; preferred checks never change the verdict; unclear counts as fail.
