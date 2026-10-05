# Plan for Issue #72: Hash Verification Error Handling

## Diagnosis

The `verify_password()` function in `core/security.py` (line 37) calls `pwd_context.verify(plain_password, hashed_password)` from passlib without catching `UnknownHashError`. When a malformed or unrecognized hash string is passed (e.g., wrong format, corrupted data), passlib raises `UnknownHashError` instead of returning `False`. This unhandled exception propagates up the call stack, causing 500 errors in upstream auth callers instead of failing closed with a simple authentication rejection.

**Root cause:** Missing exception handler for `UnknownHashError` in password verification logic.

**Evidence:** Test `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py` is marked XFAIL and documents this exact scenario:
```
passlib.exc.UnknownHashError: hash could not be identified
```

## Scope

**What will change:**
- `core/security.py`: Add try-except block in `verify_password()` to catch `UnknownHashError` and return `False`

**What won't change:**
- No changes to passlib configuration or password hashing scheme
- No changes to function signature
- No changes to test suite structure (only XFAIL marker will be removed)
- No changes to upstream callers (they already expect `False` on failed verification)

**Files affected:**
- `core/security.py` (verify_password function)

## Approach

1. Import `UnknownHashError` from `passlib.exc` at the top of `core/security.py`
2. Wrap the existing `pwd_context.verify()` call in a try-except block
3. Catch `UnknownHashError` and return `False` (fail closed, same as invalid password)
4. Keep any other exceptions to propagate (indicates real errors in configuration or environment)
5. Remove or update the XFAIL marker on the test once the fix is verified

**Implementation strategy:**
```python
from passlib.exc import UnknownHashError

def verify_password(plain_password, hashed_password):
    try:
        return pwd_context.verify(plain_password, hashed_password)
    except UnknownHashError:
        return False  # Fail closed: unrecognized hash = failed auth
```

## Test Plan

**Before the fix:**
1. Run: `pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv --runxfail --tb=short`
2. Expected: `FAILED` with `passlib.exc.UnknownHashError: hash could not be identified`

**After the fix:**
1. Run: `pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv`
2. Expected: `PASSED` (or remove XFAIL marker if test is updated to expect `False` return)

**Verification:**
- Run full test suite: `pytest tests/unit/test_security.py -v` to ensure no regressions
- Verify that other password verification tests still pass (correct password, wrong password, None hash, etc.)

## Risks and Unknowns

**Risk:** The XFAIL test marker might need to be removed or the test logic updated (currently expects exception, after fix will expect `False` return). Need to check the test code to understand its current assertions.

**Unknown:** Whether passlib raises any other hash-related exceptions that should also be caught. Current investigation suggests `UnknownHashError` is the primary concern for this issue.

**Mitigation:** Run the full test suite after the fix to catch any unexpected side effects. The fix is minimal and isolated, so regressions are unlikely.

## Deviations

### Implementation
The fix followed the plan exactly as described:
1. Imported `UnknownHashError` from `passlib.exc`
2. Wrapped `pwd_context.verify()` in try-except block
3. Returned `False` on `UnknownHashError` exception
4. Removed the XFAIL marker from `test_verify_with_wrong_hash_format`

### Test Results
- Target test now passes: `test_verify_with_wrong_hash_format` PASSED
- Full security test suite: all 25 tests PASSED
- No regressions introduced
- Commit: a68240d (branch: fix/72-verify-hash-format)

### No deviations from plan
The implementation matched the plan's approach and scope exactly. No unexpected challenges or changes were needed.
