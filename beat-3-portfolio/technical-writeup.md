# Fixing Silent Parameter Loss and Hardening Runtime Validation in Arbeit-Studio/apikit

## Overview

In Arbeit-Studio/apikit, an open-source Python library for building declarative HTTP API clients, `HTTPGatewaySpec` silently ignored unknown keyword arguments and dropped custom headers instead of forwarding them to the request gateway. At the same time, core URL and method checks relied on bare Python `assert` statements that disappear when running Python in optimized mode (`python -O`). I diagnosed the root cause, implemented strict argument validation in `apikit/specs.py`, wired header inheritance from class attributes and constructor parameters into the gateway, replaced the assertions with explicit `ValueError` guards, and expanded the test suite in `tests/test_specs.py` to 100 passing tests at 99.09% coverage. I also authored the repository-specific pre-merge manifest (`ship-manifest.md`), maintainer communication rules (`voice-guide.md`), and PR monitoring playbook (`playbook.md`). The fix is currently open upstream as [Pull Request #34](https://github.com/Arbeit-Studio/apikit/pull/34) and is awaiting maintainer review.

## What happened

The issue surfaced on GitHub as Issue #27 ("HTTPGatewaySpec silently ignores unknown arguments, including headers"). Looking into `apikit/specs.py`, `HTTPGatewaySpec.__init__` accepted `**kwargs` but never inspected, forwarded, or validated anything passed into it. If a developer made a typo like `timout=5` or passed a valid setting like `headers={"X-Version": "2"}`, the argument was absorbed into `**kwargs` and vanished without a traceback, leaving the underlying network gateway unconfigured.

Before writing any fix, I confirmed the bug on clean `main` at commit `1ab2332` using an isolated Python 3.14 environment on Ubuntu (WSL2):

```python
from apikit.protocols import HTTPMethod
from apikit.specs import HTTPGatewaySpec

spec = HTTPGatewaySpec(
    url="https://api.example.com/users",
    method=HTTPMethod.GET,
    timout=5,
    headers={"X-Custom": "value"},
)
print("timeout:", spec._gateway.timeout, "headers:", spec._gateway.headers)
```

Running the snippet produced:

```text
timeout: None headers: None
```

The typo was accepted silently, and the headers were dropped. In addition, when running with `python -O`, the bare `assert _method` check was stripped at compile time. The spec initialized cleanly with `method=None` and only failed later when calling `spec._gateway.prepare()`, crashing with `AttributeError: 'NoneType' object has no attribute 'value'`.

I created the branch `fix/27-http-gateway-spec-kwargs-headers` and made the changes in `apikit/specs.py`:
1. Removed `**kwargs` from `HTTPGatewaySpec.__init__` so Python raises standard `TypeError` exceptions for unexpected arguments.
2. Added `headers: Optional[dict] = None` as both a class attribute and constructor argument, resolving `_headers = headers or getattr(cls, "headers", None)` and passing `headers=_headers` to the underlying gateway.
3. Replaced `assert _url` and `assert _method` with explicit `ValueError` checks.

In `tests/test_specs.py`, I updated four existing tests that expected `AssertionError` to assert `ValueError`, and added four new tests covering unexpected keyword arguments, header forwarding, class inheritance, and instance overrides.

Before opening the pull request, I ran my automated quality gate, `ship-gate`. The gate runs the full test suite with upstream's 98% coverage requirement (`pytest --cov=apikit --cov-fail-under=98`) and checks diff cleanliness and plan fidelity. The tool stopped my initial run:

```text
ship-gate: STOP.
- clean-diff-quality: trailing whitespace / blank line at EOF in tests/test_specs.py
- plan-fidelity: plan stated python -O would be tested in-process via pytest, but implementation used subprocess verification
```

This stopped me from submitting a messy pull request. In-process `-O` testing is impossible in pytest because Python's `__debug__` flag is evaluated at compile time. I cleaned the trailing newline from `tests/test_specs.py`, documented the `__debug__` constraint under a dedicated `## Deviations` section in `plan.md` and `pr_draft.md`, and amended the commit (`d4eb733`).

On the second run, `ship-gate` passed cleanly:

```text
================ 100 passed, 22 warnings in 13.55s ================
TOTAL coverage: 99.09% (required fail-under: 98%)
ship-gate: PASS
```

I pushed the branch and opened upstream Pull Request #34. GitHub linked the pull request to Issue #27 automatically. Running `pr-monitor` on the open PR confirmed the branch was healthy and drafted a private wait note honoring the 7-day quiet threshold from my playbook.

## Limitations and what's next

While `HTTPGatewaySpec` now directly catches typos like `timout=5` with a `TypeError` and validates required fields unconditionally, other descriptor classes in `apikit` (such as response and serialization adapters) still accept unvalidated `**kwargs`. Custom headers are also passed through as a raw dictionary without case-insensitive HTTP header normalization.

Next, I would like to propose a shared descriptor base class to standardize argument validation across the library, and add case-insensitive header mapping to `DefaultHTTPRequestGateway`. Pull Request #34 is currently open, passing all local checks at 99.09% test coverage, and waiting for the maintainer's review.
