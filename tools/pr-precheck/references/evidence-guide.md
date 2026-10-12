# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

### Where it lives
- **Eval bundle**: Read `Plan context` (the plan's stated scope, boundary, target files, test plan, and any explicit deviation notes) side-by-side with `Candidate PR` -> `Diff` (changed files and hunks) and `Candidate PR` -> `Description` (fidelity claims).
- **Live mode**: Read `plan.md` (including its `## Deviations` section) side-by-side with `git diff main...HEAD` and the draft PR description in `pr_draft.md`.

### What good looks like
- Every file touched in the diff is explicitly listed or authorized by the plan.
- The diff contains no unannounced feature additions, new CLI flags or configuration parameters, architectural refactors, or adjacent cleanups that the plan did not specify or explicitly scoped out.
- Any reduction in scope or deferred requirement is openly acknowledged and justified in both the plan's deviation notes and the PR description (an honest shortfall/deferral).
- The PR description does not claim accomplishments or documentation updates that are absent from the actual diff.

---

## Test evidence (harness category: not-tested)

### Where it lives
- **Eval bundle**: Read `Plan context` -> `Test plan` and `Repro evidence` side-by-side with `Candidate PR` -> `Test evidence`.
- **Live mode**: Read the test plan in `plan.md` side-by-side with `test_evidence.md` (and the testing section of `pr_draft.md`).

### What good looks like
- The test evidence provides concrete, observable before/after verification (such as command output, transcripts, or test suite runs) demonstrating that the previously reproduced bug now passes or is resolved.
- Verification exercises the bug's actual triggering fault path rather than an unchanged control path or unrelated test case.
- Bare assertions without observable output (e.g., "tested locally", "works on my machine", or "tests pass" without proof) are not accepted.
- If the plan specified multiple repro cases or repository test suites, each is either shown with results or explicitly accounted for.

---

## Diff quality (harness category: unreviewable)

### Where it lives
- **Eval bundle**: Read `Candidate PR` -> `Diff` and `Candidate PR` -> `Commits`.
- **Live mode**: Read `git diff main...HEAD` and `git log main..HEAD`.

### What good looks like
- The diff is focused strictly on the bug fix and is easy for maintainers to review.
- The diff is completely free of debug debris (temporary `print`, `console.log`, or `eprintln` statements), commented-out experimental code blocks, and dead uncalled helper functions or probe variables.
- The diff is free of gratuitous noise, such as massive whitespace re-indentation passes, reordered imports, or churn across lines unrelated to the fix.

---

## Standards and comms (harness category: standards-wall)

### Where it lives
- **Eval bundle**: Read `Repo facts` (PR template instructions, checklist requirements, contributing guides, and AI-use policy) side-by-side with `Candidate PR` -> `Title` and `Candidate PR` -> `Description`.
- **Live mode**: Read `.github/PULL_REQUEST_TEMPLATE.md` and repository policies (`CONTRIBUTING.md`) side-by-side with `pr_draft.md`.

### What good looks like
- The PR description substantively completes all required template sections and checklists, including explicitly linking the issue (e.g., `Closes #<issue_number>`).
- If the repository has an explicit AI-use disclosure policy, the PR description explicitly provides a statement of AI assistance in the author's own words.
- Stated checklist items are not left unchecked or deleted when the repo mandates them.
