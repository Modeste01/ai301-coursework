# Procedure: how this tool grades a PR package

## Read order

Follow this sequence before evaluating any check:

1. **Read Repo facts / standards**: Note whether the repository specifies a required PR template, required checklist items, issue closure format (e.g. `Closes #123`), and any explicit AI-use disclosure policy.
2. **Read Issue and Thread highlights**: Note the core problem, affected components, and any guidance or constraints provided by maintainers.
3. **Read Plan context**:
   - Extract the plan's authorized scope and boundary (in-scope files and components vs out-of-scope items).
   - Extract the plan's specified test plan and repro steps.
   - Note any explicit deviation or deferral notes that justify changes from the original plan.
4. **Read Candidate PR**:
   - Inspect the `Diff` and `Commits` first to establish what was actually changed in the code.
   - Inspect `Test evidence` to see what was verified and whether before/after output is presented.
   - Inspect `Title` and `Description` to check fidelity claims, template compliance, and AI disclosure.

---

## Evidence gathering

Gather evidence for each rubric check by pairing corresponding sections:

1. **For `plan-fidelity`**:
   - Compare all changed files and hunks in the diff against the plan's scope list.
   - Check if any new CLI flags, configuration fields, unrelated refactors, or unrequested features appear in the diff.
   - Check if any planned deliverables were omitted; if so, verify whether they are documented in both plan deviation notes and the PR description as an honest deferral.
   - Compare the PR description's claims against the diff to ensure it does not claim work not delivered.
2. **For `decisive-test-evidence`**:
   - Compare the test evidence against the plan's test plan and the original bug's reproduction.
   - Verify that the evidence includes observable before/after execution (transcripts/results) on the bug's actual trigger path, rather than an unchanged control path or unsupported statements ("tested locally", "all pass").
   - Confirm all planned test scenarios or repo-required checks are evidenced or accounted for.
3. **For `clean-diff-quality`**:
   - Scan the unified diff for debug prints (`print`, `console.log`, `eprintln`), commented-out code blocks, and unused probe functions.
   - Check for formatting churn or bulk file modifications unrelated to the core fix.
4. **For `standards-and-disclosure`**:
   - Verify whether required template sections and checklists are filled and whether the issue is referenced.
   - If repo facts mandate an AI-use disclosure, verify that an explicit AI-disclosure statement is present in the PR description.

---

## Check execution

Execute the checks in rubric table order:

1. **Grade `plan-fidelity`**: Grade `pass` if the diff stays strictly within the authorized plan (or has honestly disclosed shortfalls/deferrals in deviation notes and description) and description claims match diff reality. Grade `fail` if unannounced additions, extra flags/refactors, or false claims are present.
2. **Grade `decisive-test-evidence`**: Grade `pass` if observable before/after verification on the reproduced failure path is present. Grade `fail` if evidence is missing, shows only unchanged controls, or consists of bare assertions without output.
3. **Grade `clean-diff-quality`**: Grade `pass` if the diff is clean, minimal, and reviewable. Grade `fail` if debug prints, commented-out experiments, dead probe functions, or churn are present.
4. **Grade `standards-and-disclosure`**: Grade `pass` if template sections/checklists are completed and mandatory AI disclosures are present. Grade `fail` if required template asks or AI policies are ignored.

Record a single concise line of evidence for each check quoting the deciding fact.

---

## Verdict assembly

1. Evaluate check grades against the rubric's verdict rule:
   - If all required checks (`plan-fidelity`, `decisive-test-evidence`, `clean-diff-quality`, `standards-and-disclosure`) received `pass`, the verdict is `accept`.
   - If any required check received `fail` or `unclear`, the verdict is `reject`.
2. Construct the final fenced JSON block matching the required schema:
   - `"item"` set to the PR URL or bundle id.
   - `"checks"` containing name, grade, and one-line evidence for all evaluated checks.
   - `"verdict"` set to `"accept"` or `"reject"`.
3. Output the JSON block as the very last element of the response, preceded by an optional brief summary.
