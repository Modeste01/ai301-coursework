# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Modeste01

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5987043657

# Plan for Issue #72: Hash Verification Error Handling

## Diagnosis

The `verify_password()` function in `core/security.py` calls `pwd_context.verify()` without catching `UnknownHashError` from passlib. When a malformed hash is passed, passlib raises this exception instead of returning `False`, causing unhandled 500 errors in upstream auth callers. The test `test_verify_with_wrong_hash_format` is currently marked XFAIL and documents this exact failure mode.

## Scope

**Change:** Add exception handler in `core/security.py` to catch `UnknownHashError` and return `False`.

**No changes:** Password hashing scheme, function signature, or test suite structure.

**Files affected:** `core/security.py` (one function).

## Approach

1. Import `UnknownHashError` from `passlib.exc`
2. Wrap `pwd_context.verify()` call in try-except
3. Return `False` on `UnknownHashError` (fail closed)
4. Let other exceptions propagate

## Test Plan

**Before fix:**
```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv --runxfail
```
Expected: `FAILED` with `UnknownHashError`.

**After fix:**
```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv
```
Expected: `PASSED`.

**Full suite:** `pytest tests/unit/test_security.py -v` to verify no regressions.

## Status

Implementation complete:
- Fix applied to `core/security.py` (commit a68240d)
- All 25 tests pass, no regressions
- Branch: `fix/72-verify-hash-format` (pushed to origin)

---

## Your branch

**Branch**

fix/72-verify-hash-format

**Evidence**

Before fix (with --runxfail to see failure):
```bash
$ pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv --runxfail --tb=short
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED

=================================== FAILURES ===================================
_ TestSecurity.test_verify_with_wrong_hash_format _

    def test_verify_with_wrong_hash_format(self):
        wrong_hash = "not_a_valid_bcrypt_hash"
        result = verify_password("password", wrong_hash)
        assert result is False

E   passlib.exc.UnknownHashError: hash could not be identified
```

After fix:
```bash
$ pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
======================== 1 passed in 11.13s ========================
```

Full security test suite after fix (all 25 tests):
```bash
$ pytest tests/unit/test_security.py -v
======================== 25 passed in 17.22s ========================
```

No regressions introduced.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Single full run on 2026-10-05:
- Run 1 (final): 18/20 agreement (bar was 18/20: PASS)
  - `agreement: 18/20 scored items  (bar: 18/20: PASS)`
  - Two disagreements: pkg-14 (failed on "executable-by-stranger"), pkg-20 (graded accept, should reject)

**Package analysis**

Package: pkg-05

Gold label: clear-accept
Rubric verdict: accept
Agreement: yes (✓)

Explanation: The pkg-05 plan demonstrates a clear, diagnostic approach to fixing a search indexing issue. The plan correctly identifies the root cause (missing fulltext_search index), specifies the exact scope (add index, no schema changes), and proposes a focused fix (CREATE INDEX statement). The test plan is concrete and verifiable. The rubric evaluated this as clear-accept because the plan is straightforward, grounded in code evidence, and buildable by a stranger following the provided steps.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

"Diagnosis is grounded: Plan names the root cause (a specific bug, misconfig, or missing piece in the code) and quotes or paraphrases the evidence that led to it."

This check is in the rubric because a plan that correctly identifies the root cause is more likely to produce a working fix. A plan that targets symptoms (e.g., "add validation") instead of causes will typically fix the surface problem but miss the underlying issue. By requiring diagnosis to be grounded in evidence, the rubric ensures the plan author has actually reproduced and understood the problem before attempting a fix.

**Trade-offs**

The "diagnosis-grounded" check sometimes misses plans that correctly target structural issues without naming a specific code location. For example, a plan might say "the concurrency model is wrong" without citing a line number, yet still correctly diagnose the root cause. 

The rubric gives up some flexibility for precision: it requires evidence (quoted test output, code line, error message) to ground the diagnosis, which filters out vague but potentially correct plans. This is a deliberate trade-off favoring clarity and reproducibility over breadth. The check passed on pkg-05 and all other scored packages with concrete evidence, so the trade-off appears sound for this rubric.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
