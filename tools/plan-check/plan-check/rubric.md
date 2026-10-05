# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause read against the repro evidence's actual observed behavior | The cause the plan names is something the repro evidence directly shows (a specific component, error, or behavior the steps pin down). A cause the evidence does not mention, or that contradicts the observed output, fails. | required |
| scope-bounded | The plan's In/Out scope statement read against the stated cause | The change is limited to what the cause requires. "In scope" names specific files or components; "out of scope" excludes related-but-separate behavior. A plan with no scope boundary, or one that touches behavior unrelated to the reproduced fault, fails. | required |
| symptom-vs-cause | The plan's Approach section read against the repro evidence's stated root behavior | The approach targets the code location the evidence points at, not the user-visible symptom. A plan that patches the output (e.g. catches the exception at the call site) when the evidence shows a deeper cause fails. | required |
| executable-by-stranger | The plan's Approach and Files sections together | A contributor who has not read the thread can identify which file(s) to open and what change to make. Vague direction ("find the right place", "poke around", "fix it") without named files or a traceable entry point fails. | required |
| test-plan-observable | The plan's Test plan section | The test plan names a specific command to run, or a specific test that must pass or fail, and states the observable outcome that confirms the fix. "Undo works" without a reproducible step fails; "re-run repro step 3 and confirm exit 0" passes. | required |
| comment-fits-thread | The candidate plan comment read against the thread highlights and repo facts | The comment does not contradict or ignore active maintainer guidance; if a contribution policy exists it is respected (e.g. AI disclosure, PR scope guidance). A comment that promises an arbitrary deadline or a guaranteed fix fails. | preferred |

## Verdict rule

Accept if every required check passes; preferred checks never change the verdict; unclear counts as fail on required checks.
