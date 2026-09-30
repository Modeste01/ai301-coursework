# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Modeste01

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5905663516

Hi, I would like to investigate this issue (#72) as my first contribution. I plan to set up the local test environment in my fork, run the covering unit test `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py` against malformed hash formats, and post my reproduction report once verified.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5905681646

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

  The test is reported as XFAIL:

    XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - issue #72 (manifest H-05):
  password verify raises UnknownHashError instead of returning False

  2. Run the test with xfail disabled to observe the unhandled exception:

    pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -vv --runxfail --tb=short

  ### Observed Behavior

  The test fails with an unhandled passlib.exc.UnknownHashError:

    FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - passlib.exc.
  UnknownHashError: hash could not be identified

  Specifically, core/security.py:37 calls pwd_context.verify(plain_password, hashed_password). When passed a
  malformed or unrecognized hash string, passlib raises UnknownHashError rather than returning False.

  ### Expected Behavior

  verify_password() should catch UnknownHashError and fail closed by returning False, preventing unhandled 500 errors
  in upstream auth callers.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- Run 1 (smoke test, `--limit 3`): 3/3 agreed (pkg-01 accept, pkg-02 reject, pkg-03 accept)
- Run 2 (canary slice, `--only pkg-04,pkg-06,pkg-20`): 3/3 agreed (pkg-04 reject, pkg-06 reject, pkg-20 reject)
- Run 3 (confirming full evaluation run, `--save-run`): agreement: 20/20 scored items  (bar: 18/20: PASS)

**Package analysis**

Package id: `pkg-20`
Rubric decision: `reject`
Gold label: `reject`

Explanation: `pkg-20` evaluates candidate comments on `ghostty-org/ghostty#13604`. The reproduction report is technically complete: it records the exact release build (ghostty 1.3.1 on Fedora 42), provides executable printf terminal commands querying terminal color scheme mode 2031, captures raw console sequences (`^[[?997;2n` vs `^[[?997;1n`), and demonstrates the exact logic bug in `Config.changeConditionalState`. However, the repository facts specify a strict contribution policy (`CONTRIBUTING.md` + `AI_POLICY.md`): "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance." Neither candidate comment contains an AI disclosure statement or mentions policy compliance. Under our rubric's `repo-conventions` check, failure to comply with a mandatory disclosure policy triggers a fail grade, and because all checks are required under the verdict rule, the rubric correctly rejected the package matching the gold label.

**Check rationale**

Quoted verbatim from `rubric.md`:
```markdown
| `repo-conventions` | The repo-facts contribution policy and AI policy lines read against both comments | If the repository's stated policy requires disclosing AI assistance, the comment explicitly discloses AI usage; otherwise respects repo templates and communication rules | required |
```

Rationale: We initially considered a generic check requiring candidate comments to follow general repository communication standards. However, broad phrasing caused ambiguous evaluations on packages where technical reproduction was sound but project contribution gates were breached. In open-source issue participation, repository contribution policies—such as mandatory AI disclosure in projects like Ghostty—operate as strict admission requirements. We rejected the vague formulation in favor of an explicit condition tied directly to the repository policy facts: when AI disclosure is mandated by the target project, explicit disclosure must be present; otherwise, standard thread conventions apply.

**Trade-offs**

By making `repo-conventions` a required check that strictly enforces repository policy gates like mandatory AI disclosure, the rubric gives up the ability to accept technically pristine reproductions that fail administrative policy rules (such as `pkg-20`). A report can capture perfect tracebacks, correct environment parameters, and followable steps, yet still be rejected because it omitted required policy statements. To verify that this check did not cause false rejections on packages without disclosure rules, we re-ran `--only pkg-01,pkg-04,pkg-20` as canaries. `pkg-01` (standard repo without AI policy) passed `repo-conventions` and was accepted (1/1); `pkg-04` (wrong target) remained rejected on behavior (1/1); and `pkg-20` was rejected solely on the policy gate (1/1). The full evaluation run confirmed 20/20 agreement with zero collateral regressions across all 5 categories.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
