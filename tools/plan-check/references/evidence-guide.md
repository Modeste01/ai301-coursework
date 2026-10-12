# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives:**
- In an eval bundle: the **Repro evidence** section contains the observed behavior (the exact steps, error output, or artifact). The **Candidate plan**'s opening paragraph or "Cause" / "Diagnosis" heading states the plan's cause.
- In live mode: the student's accepted repro comment on the GitHub issue thread contains the observed behavior. The draft `plan.md`'s Cause or Diagnosis section states the claimed cause.

**What good looks like:** The stated cause names a specific component, function, or behavior that the repro evidence's steps directly exercise. If the repro evidence says "calling `verify_password` with a malformed hash raises `UnknownHashError`", a grounded diagnosis names the verify function or hash-parsing path — not a generic "input validation is missing."

## Scope and boundedness

**Where it lives:**
- In an eval bundle: the **Candidate plan**'s "Scope", "In scope / Out of scope", or equivalent section. If absent, the "Files" or "Approach" section implicitly defines scope by the files it names.
- In live mode: the draft `plan.md`'s Scope section.

**What good looks like:** The scope names at least one specific file or component In scope and at least one class of change that is Out of scope. A plan that names `verify_password` in `security.py` as In scope and "refactoring the auth layer" as Out of scope is bounded. A plan that says "fix the bug" with no exclusions is not.

## Cause vs symptom targeting

**Where it lives:**
- In an eval bundle: the **Repro evidence** section (which code the steps exercise) and the **Candidate plan**'s "Approach" and "Files" sections (where the change lands).
- In live mode: the accepted repro comment (which module/function the test exercises) and the draft `plan.md`'s Approach section.

**What good looks like:** The approach's named files or functions are the same ones the repro evidence exercises, not the output layer above them. If the evidence runs a test against `verify_password` in `security.py`, a cause-targeted plan changes `security.py`, not the API route that calls it.

## Executability

**Where it lives:**
- In an eval bundle: the **Candidate plan**'s "Files", "Approach", or numbered steps section.
- In live mode: the draft `plan.md`'s Files and Approach sections.

**What good looks like:** At least one specific filename is named (not just "the relevant module"), and the change is described with enough precision that a contributor could open that file and know what to modify. "Add a format check before the hash comparison in `security.py::verify_password`" is executable. "Find the right place and fix it" is not.

## Test plan observability

**Where it lives:**
- In an eval bundle: the **Candidate plan**'s "Test plan" or "Test" section.
- In live mode: the draft `plan.md`'s Test plan section.

**What good looks like:** The test plan names either a specific CLI command to run (with its expected output or exit code) or a specific test identifier (e.g., `pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format`) and states the observable result that confirms the fix (e.g., "returns `False` instead of raising `UnknownHashError`"). "Undo works" is not observable; "Cmd+Z after the toggle round-trip removes the typed line" is.

## Comment and thread fit

**Where it lives:**
- In an eval bundle: the **Candidate plan comment** section, the **Thread highlights** section, and the **Repo facts** block (especially `contribution policy` and `bug reports` lines).
- In live mode: the draft `comment.md`, the live GitHub issue thread, and the repo's `CONTRIBUTING.md` or equivalent policy doc.

**What good looks like:** The comment does not promise a specific delivery date or guarantee a fix. If the repo's contribution policy states an AI disclosure requirement, the comment includes it. If a maintainer has given direction in the thread, the comment does not contradict it. A comment that says "I'll send a PR once the tests pass" respects uncertainty; one that says "PR up by Friday" does not.
