---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: **is this ready to submit?** A PR package is a candidate pull request (title, description, commits, diff, test evidence), read against the plan it claims to implement and the issue that plan belongs to. You do not answer from gut feel: you answer by executing the grading procedure in `procedure.md`, which applies the rubric in `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## The question

Does this candidate pull request faithfully implement its accepted plan without silent drift, with decisive test evidence, clean and reviewable diff quality, and full compliance with repository standards and disclosure policies?

## Inputs and modes

One of:

- **Live mode**: The student's own submission before opening their pull request. Gather inputs from:
  - `plan.md` (including any `## Deviations` notes) in the repository or working directory.
  - The branch's diff relative to the default branch, produced by `git diff main...HEAD` (three dots) run from the repository root.
  - The draft PR title and description from `pr_draft.md`.
  - The test evidence from `test_evidence.md`.
  - Issue context and maintainer guidance gathered live from GitHub or issue files.
  - The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and contributing/AI policies (`CONTRIBUTING.md`).
  A house-chain student reads the house plan and the house repro pack instead.
- **Eval mode**: A package bundle (a markdown file containing Repo facts, Issue, Thread highlights, Plan context, and Candidate PR). Use ONLY the bundle text as evidence. Do not fetch or read anything else, and ignore `scope.md` and `voice-guide.md` entirely (eval mode). Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything else. It names where the pull request must live and the house rules that apply there. Refuse to grade any pull request targeting a repository outside the scoped repo. If the `Repo:` line in `scope.md` still contains an unfilled placeholder (such as `<ORG>/<PATH-REVIEW-REPO>`), stop immediately without grading and tell the student to set the `Repo:` line to their Path Review repository. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md`: the student's rules for upstream communication. Check the draft PR title and description against those rules and report any rule violations in the summary. Voice rules do not alter the rubric's verdict on their own unless a rubric check specifically evaluates them. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read and connect the components:
- `rubric.md` defines the checks table and the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives and what good looks like.
- `procedure.md` defines the execution steps. Follow it as written; where the procedure is silent, report the gap in your summary rather than improvising around it.
If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, refuse to grade and report that the tool requires filled rubric and procedure files.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict. Before the block you may output a readable per-check summary. Your reply MUST end with this fenced JSON block, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first**: Never grade a check without quoting or citing the concrete fact that decided it. "Looks good" is not evidence.
- **Grade the thing, not the polish**: A terse, complete PR can be ready; a verbose, elegant PR can hide drift or missing tests. Grade actual artifacts against requirements, not formatting aesthetics.
- **The rubric decides, not you**: If a check passes by its written condition, it passes. If it fails, it fails.
- **The procedure decides how, not you**: Follow `procedure.md` step-by-step.
- **Unclear defaults to fail**: Treat `unclear` as specified in the rubric's verdict rule; where the rule is silent, an unverified or ambiguous claim is a failing grade.
