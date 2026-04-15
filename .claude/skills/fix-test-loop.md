# Skill: fix-test-loop

Use this skill when asked to fix a failing test or investigate a flaky test.

## Overview

Implements an autonomous self-healing loop that diagnoses and fixes failing tests
without requiring manual intervention at each step.

---

## The Loop

```
MAX_ITERATIONS = 10

while iteration < MAX_ITERATIONS:
  1. Run the failing test
  2. If passing → done, report success
  3. If failing → diagnose root cause, apply fix, re-run only the failing test
  4. Log: which test failed, root cause, what changed
```

Run only the specific failing test at each iteration — never the full suite.

---

## Diagnosis Framework

For each failure, identify the category before applying a fix:

### 1. Selector / Locator Issues
- Element not found, wrong locator, ambiguous match
- Investigation: inspect DOM, check for duplicate labels, verify parent scoping
- Fix: update locator using priority order (role → label/text → CSS)

### 2. Timing / Race Conditions
- Element not ready, animation not complete, async operation still running
- Investigation: check what the page state is at the point of failure
- Fix: use proper Playwright waiting (`waitFor`, `expect(...).toBeVisible()`)

### 3. Data / State Issues
- Test depends on data that doesn't exist, or previous test left unexpected state
- Investigation: check test isolation, verify setup/teardown
- Fix: create required test data, isolate the test

### 4. Environment / Build Issues
- Missing env variables, network unreachable, TypeScript errors, infrastructure failure
- Investigation: check error message for environment indicators

---

## Hard Stops — Do Not Retry

Stop immediately and report to the user if:

- **Missing environment variables** — the test cannot run without configuration
- **Network unreachable** — external dependency is down
- **TypeScript compilation errors** — the codebase doesn't compile
- **Test infrastructure failure** — Playwright itself cannot start or the browser crashes

For hard stops: report the exact error, the category, and what the user needs to do to resolve it.

---

## Iteration Log

After each iteration, log:
- Test name
- Failure category
- Root cause (one sentence)
- What was changed

Present the full log when the loop completes.
