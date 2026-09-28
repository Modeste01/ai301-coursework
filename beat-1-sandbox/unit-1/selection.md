# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

````text
Grading candidate issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
Repository: codepath/pathreview-ai301-fa26-s1 (within scope)

Checks:
- repo-active: pass (active course repository, default branch commits within 24 hours, archived: no)
- unclaimed: pass (assignees: none, linked PRs: none; peer comments permitted under Path Review classroom house rule)
- bounded-scope: pass (isolated bug in core/security.py with covering unit test in tests/unit/test_security.py; estimated effort 1 to 2 hours)
- ai-policy-permitted: pass (coursework repository permits AI-assisted development workflow)
- maintainer-responsive: pass (issue opened by course maintainer with manifest id H-05)
- clear-reproduction: pass (concrete reproduction script, pytest command, and xfail test manifest H-05 provided)

Verdict: accept

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
  "checks": [
    {
      "name": "repo-active",
      "grade": "pass",
      "evidence": "Repository is active: default branch commits within 24 hours; archived: no"
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "Assignees: none, linked PRs: none; peer comments present under Path Review classroom house rule"
    },
    {
      "name": "bounded-scope",
      "grade": "pass",
      "evidence": "Single function fix in core/security.py with existing xfail test test_verify_with_wrong_hash_format"
    },
    {
      "name": "ai-policy-permitted",
      "grade": "pass",
      "evidence": "Coursework repository permits AI-assisted workflow"
    },
    {
      "name": "maintainer-responsive",
      "grade": "pass",
      "evidence": "Issue opened by course maintainer with manifest id H-05"
    },
    {
      "name": "clear-reproduction",
      "grade": "pass",
      "evidence": "Reproduction script and pytest command provided in issue body"
    }
  ],
  "verdict": "accept"
}
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Run 1: `agreement: 16/20 scored items  (bar: 18/20: below the bar)`
- Run 2: `agreement: 18/20 scored items  (bar: 18/20: PASS)`
- Run 3: `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

- Issue ID: `issue-12` (bookwyrm-social/bookwyrm#1133)
- Rubric decision: `reject`
- Gold label: `reject`
- Reasoning: On first inspection, `issue-12` looks like a clean starter bug: it has a `good first issue` label, an active repository with recent commits and releases, no assignees or open PRs, and a simple UI feature request for reading-goal progress bars. However, the repo-facts block quotes the project's contributing guide (`CONTRIBUTING.md -> docs.joinbookwyrm.com/contributing.html`, section "Generative AI"): *"We do not accept AI-generated code or documentation. If you are unsure how something in BookWyrm works, please ask for help – we are keen to help other humans to understand and contribute to the project."* Our `ai-policy-permitted` check evaluates whether the project bans AI-assisted contributions. Because our course workflow uses AI tools, submitting code here violates project policy. The check failed, producing a `reject` verdict that agrees with the gold label.

**Check rationale**

Quoted check from `rubric.md`:
```markdown
| `bounded-scope` | Issue body: problem description, proposed changes, and task scope | The issue modifies 1 to 2 identified files or docs pages, contains no sub-task checklists or tracking tables, and has no unresolved architectural debate in the comments | required |
```

Reasoning:
I wrote this check to prevent newcomers from taking on open-ended assignments disguised as introductory tasks. Many large projects tag broad tracking tickets with `good first issue` even when the ticket actually tracks an entire subsystem overhaul (such as `issue-05` for type annotations across SymPy, or `issue-10` for tldr command tracking). By explicitly rejecting umbrella tracking lists and unsettled design debates, the check ensures that an accepted issue has clear edges where a contributor can inspect one or two files, write a test, and finish the change without getting bogged down in repo-wide architectural decisions.

**Trade-offs**

This check gives up modular first contributions that sit inside larger tracking umbrellas. In an umbrella issue like `issue-05`, a contributor could write type annotations for a single utility file and open a valid pull request. Because the check evaluates the ticket as a whole and rejects tracking lists, it filters out the parent issue completely. I accept this trade-off because sorting out which sub-items are unclaimed inside a hundred-comment thread creates unnecessary confusion on a first contribution.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **The issue's fit to your interests and to the time available:**
I work primarily in Python and prefer backend debugging with pytest. Issue #72 is estimated at 1 to 2 hours, which fits my schedule for Unit 2. The task requires updating `verify_password` in `core/security.py` to catch passlib's `UnknownHashError` and return `False` on malformed inputs rather than crashing with an unhandled exception.

2. **What the verdict identified correctly, and what you weighed that the rubric could not:**
The rubric verified that the issue touches one file, has maintainer involvement, and has no assignees or open PRs. Beyond what the rubric checks, the issue includes an existing test (`test_verify_with_wrong_hash_format` in `tests/unit/test_security.py`) tagged `xfail` under manifest `H-05`. This lets me immediately reproduce the failure locally without setting up Docker or external databases.

3. **The anticipated difficulty in claiming it:**
Several students have commented with reproduction logs. Under standard open-source conventions, multiple active comments might signal a contested issue. Under Path Review classroom rules, peer comments do not block an issue, and credit is awarded for opening a PR. My main challenge will be writing a precise claim comment and ensuring the exception handler only catches the expected hash error without masking unexpected failures.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
