# Contribution: HTTPGatewaySpec silently ignores unknown arguments, including headers

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Modeste Houenou
**GitHub Username:** Modeste01
**Issue:** https://github.com/Arbeit-Studio/apikit/issues/27
**Pull Request:** https://github.com/Arbeit-Studio/apikit/pull/34

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and calibrated issue-scout to an early-career Python profile**

I built issue-scout with a fit profile targeting backend logic and pytest-driven bugs (input validation, CLI edge cases, error-handling gaps) in isolated Python files, and a search strategy that traces each query to a profile clause. Two things fell short in the first real run: `gh search issues` silently returned empty results whenever a quoted multi-term `"A OR B OR C"` query was combined with `--label`/`--language` (it works alone, not combined), and once candidates were graded, the rubric's `unclaimed` check caught a repo-level collision I hadn't anticipated, a curated-portal pick (Apache Mahout #1506) that looked strong on paper but had an open competing PR (#1520) already fixing it. I split every multi-term query into single-term calls unioned before ranking (logged in `scan-notes.md`), and let the rubric's `unclaimed` check do its job rather than trusting a candidate's apparent fit, which is how it rejected Mahout #1506 and left `apikit#27` as the clean accept.

**Unit 6: Installed voice-guard PreToolUse hook, patched issue-scout to scope-feasible, and built CLAUDE.local.md**

I installed `voice-guard` (`~/.claude/voice-guard/check_voice.py`) as a Claude Code `PreToolUse` hook backed by an extended `voice-guide.md` covering upstream maintainer communication (`Claims I post`, `Questions I ask`, `Pings I send`), updated `repro-check` and `plan-check` to their wild scopes, and patched `issue-scout/rubric.md` from the rigid 1–2 file `bounded-scope` rule to `scope-feasible` with a difficulty note (`straightforward`, `moderate`, `ambitious`). In the wiring test, `voice-guard` immediately stopped `gh issue comment --help` (`no body the guard can read`) and logged the interception to `~/.claude/voice-guard/guard-log.md`, then passed and logged my real claim comment (`--body-file claim-draft.md`) when posting to `Arbeit-Studio/apikit#27`. After forking and cloning `Arbeit-Studio/apikit`, I distilled the repo's Poetry/`pytest` 98%-coverage gate (`--cov=apikit --cov-fail-under=98`), commit conventions, and the `specs.py` -> `default.py` -> `protocols.py` subsystem map into a 27-line untracked `CLAUDE.local.md` excluded via `.git/info/exclude`.

**Unit 7: Configured ship-gate with coverage manifest, audited plan with plan-check, and enforced pre-PR gate**

I installed `ship-gate` (`~/.claude/ship-gate/check_gate.py`) and configured `ship-manifest.md` to run `.venv/bin/pytest tests --cov-report term --cov=apikit --cov-fail-under=98` matching upstream CI. Before coding, I graded `plan.md` and `plan-comment.md` with `plan-check` (verdict: `accept`). When running `ship-gate` on branch `fix/27-http-gateway-spec-kwargs-headers`, the gate's `pr-precheck` hook caught two real friction points: a trailing blank line at EOF in `tests/test_specs.py`, and an undisclosed plan deviation regarding the in-process `python -O` test (infeasible due to `__debug__` compile-time constant). I amended the commit to strip whitespace, documented the subprocess verification in `plan.md` and `pr_draft.md`, and re-ran `ship-gate`, which passed and cleared the pull request for creation as PR #34.

**Unit 8: Configured PR-monitor with playbook rules and evaluated live PR state**

I configured `pr-monitor` (`~/.claude/pr-monitor/monitor.py`) backed by a repository-grounded `playbook.md` with a 7-day staleness threshold derived from historical response times in `Arbeit-Studio/apikit` merged PRs (#7 and #8). When executed against my branch for PR #34, the monitor read the live GitHub PR state (0 reviews, 0 comments, quiet since opening) and correctly drafted a wait note advising against premature nudging. Because the thread had only been open for under a day, I followed the playbook's guidance and kept the wait note private rather than posting an unnecessary ping.

---

## Why I Chose This Issue

My scout surfaced `apikit#27` ("HTTPGatewaySpec silently ignores unknown arguments, including headers") from a scan that unioned single-term searches against my fit-profile queries and ranked 272 candidates down to a top 5. It graded `accept` with a difficulty note of **`straightforward`**: `HTTPGatewaySpec.__init__` takes `**kwargs` and silently drops unrecognized names, so a typo like `timout=5` or an unsupported `headers` argument just vanishes with no error, which is exactly the input-validation and error-handling-gap territory my profile targets. The fix is isolated to `apikit/specs.py` and `tests/test_specs.py`, the maintainer's issue body includes a runnable repro snippet showing the actual broken output, it's a pure Python library with no asyncio or C-extension traps, and both at initial scan time and when re-graded before claiming on 2026-11-11 it had zero comments, no assignee, and no linked or competing PR. It's a smaller, newer-looking repo (2 stars) than I'd first assumed I wanted, but the issue's author is the project's own sole recent committer, so the maintainer is clearly active and engaged with their own tracker.

I did consider a curated-portal candidate first, Apache Mahout `#1506` (a prestigious, well-starred project), as well as the other accepted scan survivor, `AdityaPainuli/clippings-vids#180`. Running them through the rubric caught collisions that a glance at star count or labels would have missed: on Mahout `#1506`, PR `#1520` (and later PR `#1567`) was already open against that exact issue, and the thread showed a maintainer reserving it for an in-person hackathon; on `clippings-vids#180`, re-grading before claiming caught that another contributor (`Harinig-prog`) had been assigned the issue with an active claim. The rubric's `unclaimed` check rejected both on live evidence, which is the tool doing its job, not a shortcoming; rejecting against appearances is precisely why I built verification into the pipeline instead of eyeballing star counts. On the rubric side, I also patched `bounded-scope` (which rejected anything over 1–2 files) to `scope-feasible` plus a difficulty note (`straightforward` / `moderate` / `ambitious`) so wild multi-file fixes aren't discarded on file count alone.

Claim comment posted on GitHub: https://github.com/Arbeit-Studio/apikit/issues/27#issuecomment-6816045005

**Toolkit**

- Tool: `issue-scout` (my search strategy in `search-strategy.md`, my patched rubric in `rubric.md`), run first as a live-mode scan over `gh search issues` results and then re-run in direct mode on `apikit#27`, `clippings-vids#180`, and `apache/mahout#1506` after patching `bounded-scope` to `scope-feasible`
- What it produced: `apikit#27` graded `accept` on all 4 required checks (`repo-active`, `unclaimed`, `scope-feasible` with difficulty note **`straightforward`**, `ai-policy-permitted`) and `clear-reproduction`; both `clippings-vids#180` (assigned to `Harinig-prog`) and `apache/mahout#1506` (competing open PRs `#1520` and `#1567`) graded `reject` on `unclaimed`
- Difficulties: `gh search issues` silently returns `[]` for a quoted multi-term OR query combined with `--label`/`--language`; and the Unit 1 `bounded-scope` check was too rigid for wild repos where a single logical fix can span implementation and test files across a module boundary
- What changed: split every multi-term query in `search-strategy.md` into single-term queries unioned by `jq` before ranking, and patched `rubric.md` from `bounded-scope` to `scope-feasible` with a `straightforward` / `moderate` / `ambitious` difficulty note (both logged in `scan-notes.md`)

---

## Understanding the Issue

**Problem Description**

`HTTPGatewaySpec` in `apikit/specs.py` is a descriptor class used to declare typed HTTP endpoints on a client class. Three input-validation and parameter-forwarding defects exist in `HTTPGatewaySpec.__init__` (`apikit/specs.py:51-111`):
1. `__init__` accepts a catch-all `**kwargs` parameter (`apikit/specs.py:65`) and never validates, forwards, or rejects it, so any unknown or misspelled keyword argument (such as `timout=5` instead of `timeout=5`) is silently discarded.
2. Even though `DefaultHTTPRequestGateway.__init__` (`apikit/default.py:124-141`) and the `HTTPRequestGateway` protocol (`apikit/protocols.py:121-131`) already accept `headers: Optional[dict] = None`, `HTTPGatewaySpec` does not declare a `headers` class attribute or `__init__` parameter and never passes `headers` when constructing `self._gateway = _gateway(...)` (`apikit/specs.py:104-111`). As a result, `headers={"X-Custom": "value"}` passed to `HTTPGatewaySpec` falls into `**kwargs` and is silently lost.
3. Required parameter checks on `apikit/specs.py:86-87` use bare `assert _url, "url must be provided"` and `assert _method, "method must be provided"`, which are stripped completely when Python runs with optimization enabled (`python -O`) and raise `AssertionError` instead of `ValueError`.

**Expected Behavior**

1. Passing an unrecognized keyword argument (e.g., `timout=5`) to `HTTPGatewaySpec(...)` immediately raises Python's standard `TypeError: HTTPGatewaySpec.__init__() got an unexpected keyword argument 'timout'`.
2. `HTTPGatewaySpec` supports `headers: Optional[dict] = None` both as a class-level attribute (inheritable by subclasses) and as an `__init__` keyword argument (with `__init__` taking priority over the class attribute), and forwards `headers=_headers` to `_gateway(...)` so `spec._gateway.headers` and outgoing `session.request(...)` calls receive the configured headers.
3. Missing or empty `url` or `method` raises `ValueError("url must be provided")` or `ValueError("method must be provided")` unconditionally, even when running under `python -O`.

**Current Behavior**

1. Passing `timout=5` and `headers={"X-Custom": "value"}` to `HTTPGatewaySpec(url="https://api.example.com/users", method=HTTPMethod.GET, timout=5, headers={"X-Custom": "value"})` succeeds without error and leaves both `spec._gateway.timeout` and `spec._gateway.headers` set to `None`.
2. Missing `url` or `method` raises `AssertionError` in normal execution, and under `python -O`, `HTTPGatewaySpec(url="https://api.example.com/users", method=None)` succeeds at construction time (`spec._gateway.method` is `None`) and only crashes later during `spec._gateway.prepare()` with `AttributeError: 'NoneType' object has no attribute 'value'`.

**Affected Components**

- `apikit/specs.py` (`HTTPGatewaySpec` class attributes on lines 39–50, `HTTPGatewaySpec.__init__` signature and body on lines 51–111)
- `apikit/default.py` (`DefaultHTTPRequestGateway.__init__` on lines 124–142 and `DefaultHTTPRequestAdapter.adapt` on lines 85–106 — inspected to verify downstream `headers` handling; already supports `headers`)
- `apikit/protocols.py` (`HTTPRequestGateway.__init__` on lines 121–131 — already includes `headers: Optional[dict] = None`)
- `tests/test_specs.py` (existing `AssertionError` tests on lines 18–35 and timeout/inheritance tests on lines 244–329, where new tests for unknown kwargs, `headers` forwarding/inheritance, and `ValueError` validation belong)

**Toolkit**

- Tool: `CLAUDE.local.md` (untracked repo-memory file distilled from `skill/claude-local-template.md` and excluded via `.git/info/exclude`) plus `repro-check` and `voice-guard` for the upstream claim comment
- What it produced: a 27-line `CLAUDE.local.md` mapping the `apikit/specs.py` -> `apikit/default.py` -> `apikit/protocols.py` -> `tests/test_specs.py` call chain and CI coverage gate (`--cov=apikit --cov-fail-under=98`), and an accepted `claim-draft.md` posted to GitHub (`#issuecomment-6816045005`) after passing both `repro-check` and the `voice-guard` `PreToolUse` hook
- Difficulties: `Arbeit-Studio/apikit` has no `CONTRIBUTING.md`, so commit style, branch conventions, and test coverage expectations had to be inferred directly from `.github/workflows/python-package.yml` and recent merged PRs (`#11`, `#8`, `#6`)
- What changed: recorded the inferred conventions and the 98% coverage requirement in `CLAUDE.local.md` so every subsequent Claude session in the `apikit` clone loads them automatically without polluting `git status`

---

## Reproduction Process

**Environment Setup**

- **OS:** Ubuntu Linux on WSL2 (`Linux 6.18.58.1-microsoft-standard-WSL2 x86_64`)
- **Python & Package Versions:** Python `3.14.5` (within the repo's CI matrix of Python `3.9`–`3.14`), `apikit 0.1.10` (editable install), `pydantic 2.12.5`, `requests 2.32.5`, `requests-cache 1.2.1`, `flask 3.1.2`, `pytest 9.2.0`, `pytest-cov 7.1.0`, `responses 0.25.8`, `pytest-responses 0.5.1`
- **Setup Commands:** Forked `Arbeit-Studio/apikit` to `Modeste01/apikit`, cloned at `main` commit `1ab23325d6a94253a523a462927607dd00959e20`, created an isolated virtual environment (`python3 -m venv .venv`), and installed the package and dev dependencies (`pip install -e . pytest responses pytest-responses pytest-cov flask`). Ran the existing test suite (`pytest tests --cov-report term --cov=apikit --cov-fail-under=98`), which passed all 96 tests at `99.07%` coverage in 2.67s. No setup blockers occurred.

**Steps to Reproduce**

1. In the cloned `apikit` repository at commit `1ab23325d6a94253a523a462927607dd00959e20`, run the issue's snippet passing both a misspelled keyword argument (`timout=5`) and `headers={"X-Custom": "value"}`:
   ```bash
   python -c '
   from apikit.specs import HTTPGatewaySpec
   from apikit.protocols import HTTPMethod

   spec = HTTPGatewaySpec(
       url="https://api.example.com/users",
       method=HTTPMethod.GET,
       timout=5,
       headers={"X-Custom": "value"},
   )
   print("timeout:", spec._gateway.timeout, "headers:", spec._gateway.headers)
   '
   ```
2. Run Python with `-O` (optimization enabled, stripping `assert` statements) when `method=None`:
   ```bash
   python -O -c '
   from apikit.specs import HTTPGatewaySpec
   spec = HTTPGatewaySpec(url="https://api.example.com/users", method=None)
   print("Initialized under python -O with method=None! spec._gateway.method =", spec._gateway.method)
   try:
       spec._gateway.prepare()
   except Exception as e:
       print("Crash deferred to prepare():", type(e).__name__, e)
   '
   ```
3. Observe that Step 1 prints `timeout: None headers: None` with no exception raised, and Step 2 constructs `HTTPGatewaySpec` with `spec._gateway.method = None` before failing inside `prepare()` with `AttributeError: 'NoneType' object has no attribute 'value'`.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/Modeste01/apikit/commit/1ab23325d6a94253a523a462927607dd00959e20
- **Screenshots/logs:**
  ```text
  $ pytest tests --cov-report term --cov=apikit --cov-fail-under=98
  96 passed, 22 warnings in 2.67s (Total coverage: 99.07%)

  $ python -c '... print("timeout:", spec._gateway.timeout, "headers:", spec._gateway.headers)'
  timeout: None headers: None

  $ python -O -c '... HTTPGatewaySpec(url="https://api.example.com/users", method=None) ...'
  Initialized under python -O with method=None! spec._gateway.method = None
  Crash deferred to prepare(): AttributeError 'NoneType' object has no attribute 'value'
  ```
- **My findings:** All three parts of issue `#27` reproduced cleanly on `main` (`1ab2332`). Inspection of `apikit/default.py` and `apikit/protocols.py` confirmed that `DefaultHTTPRequestGateway` and `DefaultHTTPRequestAdapter` already wire `headers` all the way through to `session.request(method=..., url=..., headers=headers, timeout=..., params=...)` (in fact, `tests/test_specs.py:266,297,325` explicitly assert `headers=None` in `mock_request.assert_called_once_with(...)` because `HTTPGatewaySpec` never forwarded `headers`). Also noted that four existing tests in `tests/test_specs.py:18-35` currently assert `pytest.raises(AssertionError)` for missing/None `url` and `method` and will need to be updated to `pytest.raises(ValueError)` alongside the fix in Unit 7.

**Toolkit**

- Tool: `repro-check` (with wild scope `repro-check-scope-wild.md` and symlinked `voice-guide.md`) and `voice-guard` (`check_voice.py --draft`), run against `claim-draft.md` and `repro-draft.md`
- What it produced: `repro-check` graded the combined claim + reproduction package `accept` across all 6 required checks (`environment-recorded`, `steps-followable`, `artifact-faithful`, `behavior-matches`, `claim-modest-and-specific`, `repo-conventions`), and `voice-guard` passed both drafts with zero rule violations
- Difficulties: none met during local reproduction; the bug is deterministic and reproducible in a plain Python venv without external services
- What changed: nothing needed in `repro-check`'s rubric; kept `repro-draft.md` saved locally for folding into the Unit 7 PR description rather than double-posting on the quiet upstream issue thread

---

## Solution Approach

**Analysis**

In `apikit/specs.py`, `HTTPGatewaySpec.__init__` exhibits three root-cause validation and forwarding defects:
1. `HTTPGatewaySpec.__init__` accepts `**kwargs` (`apikit/specs.py:65`) without validation or forwarding. Misspelled keyword arguments like `timout=5` are silently absorbed.
2. `DefaultHTTPRequestGateway.__init__` (`apikit/default.py:124-141`) already accepts `headers: Optional[dict] = None`, but `HTTPGatewaySpec` neither defines a `headers` class attribute nor an `__init__` parameter, omitting `headers` from its `_gateway(...)` call (`apikit/specs.py:104-111`).
3. Lines 86–87 rely on bare `assert _url` and `assert _method`. When Python runs with `-O`, assertions are stripped at compile/runtime, deferring crashes to `_gateway.prepare()` with `AttributeError: 'NoneType' object has no attribute 'value'`.

**Proposed Solution**

- **In Scope:**
  - `apikit/specs.py`:
    1. Define class attribute `headers: Optional[dict] = None` on `HTTPGatewaySpec`.
    2. Remove `**kwargs` from `HTTPGatewaySpec.__init__` so Python's keyword-only signature automatically raises `TypeError` on unrecognized arguments.
    3. Add `headers: Optional[dict] = None` to `HTTPGatewaySpec.__init__` and resolve `_headers = headers or getattr(cls, "headers", None)`.
    4. Pass `headers=_headers` when instantiating `self._gateway = _gateway(...)`.
    5. Replace bare `assert _url` and `assert _method` with explicit `if not _url: raise ValueError("url must be provided")` and `if not _method: raise ValueError("method must be provided")`.
  - `tests/test_specs.py`:
    1. Update the 4 existing tests (lines 18–35) that assert `AssertionError` for missing/None `url` and `method` to assert `pytest.raises(ValueError)`.
    2. Add unit tests for unexpected kwargs (`TypeError`), headers forwarding, and class-level headers inheritance/overrides.
- **Out of Scope:**
  - `apikit/default.py` and `apikit/protocols.py` (inspected; already accept and wire `headers` properly).
  - `apikit/session.py` or unrelated subsystems.

**Implementation Plan**

1. Edit `apikit/specs.py` to add `headers` class attribute, remove `**kwargs`, add `headers` parameter, pass `headers=_headers` to `_gateway(...)`, and replace `assert` with `ValueError`.
2. Update existing tests in `tests/test_specs.py` from `AssertionError` to `ValueError`.
3. Add new unit tests in `tests/test_specs.py` for unknown kwargs (`TypeError`), headers forwarding, and headers class-attribute override.
4. Run full test suite with coverage gate: `.venv/bin/pytest tests --cov-report term --cov=apikit --cov-fail-under=98`.

**Deviations Recorded During Implementation:**
- *In-process `-O` test vs. subprocess verification:* `__debug__` is a Python compile-time constant, so pytest cannot simulate `-O` in-process without spawning a child interpreter. Because `apikit/specs.py` uses explicit `if not _url:` / `if not _method:` raises, normal pytest execution thoroughly exercises the `ValueError` logic. Direct execution with `python -O` was verified via subprocess in `test_evidence.md`.
- *Additional test added:* Added `test_http_gateway_spec_init_headers_override_class_attribute` to ensure instance arguments take precedence over class-level defaults.

**Toolkit**

- Tool: `plan-check` (with wild scope `plan-check-scope-wild.md`), run against `plan.md` and draft `plan-comment.md`
- What it produced: `Verdict: accept` on all required checks (`diagnosis-grounded`, `scope-bounded`, `symptom-vs-cause`, `executable-by-stranger`, `test-plan-observable`, `comment-fits-thread`)
- Difficulties: `plan-check` surfaced that in-process `-O` testing is impossible via pytest due to Python's compile-time `__debug__` constant, and advised testing it via subprocess verification
- What changed: updated the plan and PR draft to document the subprocess verification under Deviations instead of attempting an infeasible in-process test fixture

---

## Testing Strategy

The fix was verified through three layers of observable checks:

1. **Reproduction Snippets Re-run (Before vs. After):**
   - *Typo keyword argument:* `.venv/bin/python -c '... HTTPGatewaySpec(url=..., method=..., timout=5) ...'`
     - Before: Silently accepted; printed `timeout: None headers: None`.
     - After: Immediately raised `TypeError: HTTPGatewaySpec.__init__() got an unexpected keyword argument 'timout'. Did you mean 'timeout'?`.
   - *Headers forwarding:* `.venv/bin/python -c '... HTTPGatewaySpec(url=..., method=..., headers={"X-Custom": "value"}) ...'`
     - Before: Dropped headers; printed `headers: None`.
     - After: Successfully forwarded; printed `Forwarded headers: {'X-Custom': 'value'}`.
   - *Optimization `-O` validation:* `.venv/bin/python -O -c '... HTTPGatewaySpec(url=..., method=None) ...'`
     - Before: Initialized with `method=None` and crashed later in `prepare()` with `AttributeError`.
     - After: Immediately raised `ValueError: method must be provided`.

2. **Targeted Unit Tests in `tests/test_specs.py`:**
   - Updated 4 existing tests (`test_http_gateway_spec_init_with_none_url`, `test_http_gateway_spec_init_with_no_url`, `test_http_gateway_spec_init_with_none_method`, `test_http_gateway_spec_init_no_method`) to assert `pytest.raises(ValueError, match=...)`.
   - Added `test_http_gateway_spec_init_with_unexpected_kwarg` verifying `TypeError` for unknown keyword arguments.
   - Added `test_http_gateway_spec_init_with_headers` verifying headers assignment to `_gateway.headers`.
   - Added `test_http_gateway_spec_inheritance_with_headers_attribute` and `test_http_gateway_spec_init_headers_override_class_attribute` verifying subclass inheritance and instance override behavior.

3. **Full Test Suite & Coverage Gate:**
   - Ran `.venv/bin/pytest tests --cov-report term --cov=apikit --cov-fail-under=98`.
   - Outcome: 100 passed (out of 100), 22 warnings, total coverage: **99.09%** (exceeding the repository's 98% required CI gate).

**Toolkit**

- Tool: `pytest` and `pytest-cov` executed in local Python 3.14 virtual environment matching the project's CI configuration
- What it produced: 100 passing unit tests and 99.09% coverage report across all `apikit` modules
- Difficulties: none; the test suite is fast (13.55s) and deterministic
- What changed: captured complete before-and-after terminal output into `test_evidence.md` for consumption by `ship-gate` and inclusion in the PR description

---

## Implementation Notes

**2026-11-12**

Forked `Arbeit-Studio/apikit`, cloned locally, and created untracked `CLAUDE.local.md` excluded via `.git/info/exclude`. Established local Python 3.14 environment and confirmed that all 96 existing tests passed at 99.07% coverage. Verified the three reported failure mechanisms: `timout=5` and `headers` silently dropped into `**kwargs`, and `python -O` stripping `assert _method`. Posted verified claim comment on GitHub (`#issuecomment-6816045005`).

**2026-11-18**

Installed `ship-gate` and configured `ship-manifest.md`. Drafted `plan.md` and `plan-comment.md`, audited with `plan-check` (`Verdict: accept`), and branched `fix/27-http-gateway-spec-kwargs-headers`. Implemented the fix in `apikit/specs.py` (+8/-3 lines) and updated/added tests in `tests/test_specs.py` (+39/-4 lines). 

Ran `ship-gate`, which caught two real issues before PR creation: (1) an extraneous blank line at EOF in `tests/test_specs.py`, and (2) an undocumented plan deviation regarding why `-O` is verified via subprocess rather than an in-process pytest test. Amended the commit to clean the diff, documented the deviation in `plan.md` and `pr_draft.md`, and re-ran `ship-gate`, which passed all checks (`ship-gate: PASS`). Pushed branch `fix/27-http-gateway-spec-kwargs-headers` to fork and opened PR #34 on `Arbeit-Studio/apikit`.

**2026-11-18 (Continued)**

Followed through on the original Nov 12 claim commitment by running a courtesy check-in draft through `voice-guard` and posting comment #6994055895 on issue #27 reporting local reproduction environment and linking PR #34. Installed `ai301-unit8-starter` materials, authored `~/.claude/pr-monitor/playbook.md` with response rules, staleness thresholds, nudge etiquette, and walk-away criteria calibrated to `apikit`'s historical PR merge cadence. Ran `pr-monitor --state` and a full run on PR #34; the tool generated `run-report.md` and `drafted-move.md`, recommending a quiet wait note in full agreement with the 7-day playbook threshold.

**Toolkit**

- Tool: Git CLI (`git switch -c`, `git commit --amend`), Python virtualenv, and `ship-gate` (`check_gate.py`)
- What it produced: Clean two-file commit `d4eb733` (+46/-7 lines), clean test run (100 passed, 99.09% cov), and successful PR creation
- Difficulties: `ship-gate` strictly rejected the branch on the first pass due to trailing whitespace and missing deviation documentation
- What changed: fixed the formatting in `tests/test_specs.py` and updated `plan.md` and `pr_draft.md` with explicit deviation disclosures

---

## Pull Request

**PR Link:** https://github.com/Arbeit-Studio/apikit/pull/34

**PR Description:**

```markdown
Fixes #27

### Summary of Changes

This pull request resolves parameter-handling and validation defects in `HTTPGatewaySpec` (`apikit/specs.py`):
1. **Removes `**kwargs` from `HTTPGatewaySpec.__init__`**: Unknown or misspelled arguments (such as `timout=5`) now immediately raise Python's standard `TypeError: HTTPGatewaySpec.__init__() got an unexpected keyword argument 'timout'` instead of being silently ignored.
2. **Adds `headers` support**: Defines `headers: Optional[dict] = None` as a class attribute and `__init__` parameter, properly resolving `_headers = headers or getattr(cls, "headers", None)` and passing `headers=_headers` to the underlying gateway instance (`_gateway(...)`).
3. **Replaces `assert` checks with `ValueError`**: `url` and `method` validation in `HTTPGatewaySpec` now raises `ValueError` if missing or empty, ensuring validation is not stripped when running under `python -O`.
4. **Test coverage**: Updates existing tests expecting `AssertionError` to `ValueError`, and adds unit tests covering unexpected kwargs (`TypeError`), headers forwarding, and class-level headers inheritance.

### Environment

- **OS / Runtime:** Ubuntu Linux (WSL2), Python 3.14.5, `pytest 9.2.0`, `pytest-cov 7.1.0`
- **Base Commit:** Tested against `main` head at `1ab23325d6a94253a523a462927607dd00959e20` (v0.1.10, no version delta)

### Verification and Test Evidence

**Before the fix:**
- Unknown kwargs and headers were dropped without error:
  `timeout: None headers: None`
- Missing `method` under `python -O` bypassed assertion and failed later in `prepare()`:
  `Crash deferred to prepare(): AttributeError 'NoneType' object has no attribute 'value'`

**After the fix:**
- Typo keyword argument raises expected `TypeError`:
  `Caught expected TypeError for timout=5: HTTPGatewaySpec.__init__() got an unexpected keyword argument 'timout'. Did you mean 'timeout'?`
- Configured headers are forwarded to `spec._gateway.headers`:
  `Forwarded headers: {'X-Custom': 'value'}`
- Missing `method` under `python -O` raises `ValueError` immediately:
  `Caught expected ValueError under python -O: method must be provided`
- Full test suite passing with 99.09% coverage (exceeding required 98% gate):
  `100 passed, 22 warnings in 13.55s (Total coverage: 99.09%)`

### Deviations from Initial Plan

- **`-O` test verification**: Initial planning considered an in-process pytest test for `python -O`. Because `__debug__` is a Python compile-time constant, pytest cannot simulate `-O` in-process. Replacing `assert` with explicit `if not _url:` / `if not _method:` raises guarantees that the checks run unconditionally under all optimization levels, which was verified via subprocess in `test_evidence.md`.
- **Extra test added**: Added `test_http_gateway_spec_init_headers_override_class_attribute` to ensure instance arguments override class-level defaults.

### AI Assistance Disclosure

Claude Code assisted with inspecting parameter propagation through `DefaultHTTPRequestGateway`, drafting minimal reproducer snippets, and checking coverage requirements against CI. All logic changes, tests, and verifications were reviewed, executed, and confirmed locally.
```

**Maintainer Feedback:**

- 2026-11-18: none yet

**Status:** Awaiting review

**Toolkit**

- Tool: `ship-gate` (`~/.claude/ship-gate/check_gate.py`) and `voice-guard` (`~/.claude/voice-guard/check_voice.py`), run before PR creation via Claude Code
- What it produced: automated verification pass (`ship-gate: PASS`) confirming repo tests (100 passed, 99.09% coverage) and `pr-precheck` quality gates before submitting PR #34 to `Arbeit-Studio/apikit`
- Difficulties: ensuring `gh repo set-default` pointed at `Arbeit-Studio/apikit` so the PR was opened against the upstream repository rather than the personal fork
- What changed: ran `gh repo set-default Arbeit-Studio/apikit` prior to invocation, ensuring Claude Code opened PR #34 directly on the upstream project

---

## Learnings and Reflections

**Technical Skills Gained**

I deepened my understanding of Python descriptor protocols and parameter forwarding by tracing how `HTTPGatewaySpec` binds and forwards attributes to `DefaultHTTPRequestGateway` and underlying `session.request` invocations. I mastered strict keyword argument handling in Python class constructors: omitting catch-all `**kwargs` allows Python to enforce keyword validity automatically via `TypeError` rather than letting misspelled parameters silently vanish. Additionally, I learned the critical runtime implications of Python optimization flags: because `__debug__` is evaluated at compile time, bare `assert` statements are stripped when running under `python -O`, requiring explicit `ValueError` validation checks for unconditional production enforcement. Finally, I gained practical experience maintaining test suites under strict coverage gates (`pytest --cov-fail-under=98`), designing edge-case unit tests for unexpected arguments, headers forwarding, and subclass inheritance.

**Challenges Overcome**

The biggest tooling challenge was diagnosing why `gh search issues` returned empty results during initial scanning: queries combining quoted multi-term OR expressions with `--label` and `--language` filters silently drop all matches in GitHub's search API. I overcame this by restructuring the search strategy into discrete single-term queries unioned with `jq`. On the workflow side, `ship-gate` rejected my initial PR branch for a trailing blank line at EOF in `tests/test_specs.py` and flagged an undisclosed deviation between my planned in-process `-O` test and the subprocess verification necessitated by `__debug__`'s compile-time semantics. I resolved this by cleaning the diff, documenting the subprocess testing rationale in `plan.md` and `pr_draft.md`, and clearing all automated gate checks before submission. When drafting upstream comments, `voice-guard` caught drafts that omitted exact dependency and environment deltas, ensuring all public communication remained grounded in verified artifacts.

**What I'd Do Differently Next Time**

Next time, I will investigate test runtime constraints and optimization mechanics during initial plan formulation rather than during implementation, recognizing early that Python's `__debug__` constant prevents in-process optimization testing. I would also establish repository conventions in `CLAUDE.local.md` before writing the first line of code to eliminate formatting and whitespace friction during pre-PR gate checks.

---

## Resources Used

- Arbeit-Studio/apikit Issue #27 ("HTTPGatewaySpec silently ignores unknown arguments, including headers"): https://github.com/Arbeit-Studio/apikit/issues/27
- Arbeit-Studio/apikit Pull Request #34 ("Validate arguments and forward headers in HTTPGatewaySpec"): https://github.com/Arbeit-Studio/apikit/pull/34
- Python Language Reference documentation on the `assert` statement and `__debug__`: https://docs.python.org/3/reference/simple_stmts.html#the-assert-statement
- Python Tutorial documentation on Keyword Arguments and Function Annotations: https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments
- pytest-cov Documentation on Configuration and `--cov-fail-under`: https://pytest-cov.readthedocs.io/en/latest/config.html
