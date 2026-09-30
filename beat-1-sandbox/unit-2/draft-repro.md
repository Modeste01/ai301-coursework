I reproduced this issue on Ubuntu 24.04 (WSL2) on clean main at commit f89c06f.

### Environment
- OS: Ubuntu 24.04.1 LTS (Linux 5.15 x86_64 via WSL2)
- Python: 3.12.3 (inside project `.venv`)
- pytest: 9.1.1
- passlib: 1.7.4
- bcrypt: 4.3.0
- Repo commit: f89c06f

### Reproduction Steps
1. In repository root with the project virtual environment activated, run the covering unit test:
```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv -rxX
```
The test is reported as `XFAIL`:
```text
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
```

2. Run the test with xfail disabled to observe the unhandled exception:
```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv --runxfail --tb=short
```

### Observed Behavior
The test fails with an unhandled `passlib.exc.UnknownHashError`:
```text
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - passlib.exc.UnknownHashError: hash could not be identified
```
Specifically, `core/security.py:37` calls `pwd_context.verify(plain_password, hashed_password)`. When passed a malformed or unrecognized hash string, `passlib` raises `UnknownHashError` rather than returning `False`.

### Expected Behavior
`verify_password()` should catch `UnknownHashError` and fail closed by returning `False`, preventing unhandled 500 errors in upstream auth callers.

*Note: Prepared with AI assistance per course guidelines; reproduction commands and outputs were executed and verified locally by me.*
