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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
