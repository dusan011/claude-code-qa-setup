# CLAUDE.md — QA Automation Project

This file is read automatically by Claude Code at the start of every session.
It defines project conventions, critical rules, and standards that apply to all tasks.

---

## Project Overview

- **Framework:** Playwright (TypeScript)
- **Pattern:** Page Object Model (POM)
- **Auth:** Session-based via `storageState` — auth is handled by a setup project, not individual tests
- **Test runner:** Playwright test runner with project-based configuration

---

## Authentication — Critical Rules

This project uses Playwright's `storageState` to handle authentication.
The setup project runs once, saves the session to disk, and every test loads it automatically.

> **DO NOT call the programmatic login helper inside individual tests.**
> Each browser context already has the session loaded from storageState.

If a test requires a different user role, ask before modifying auth logic.

---

## E2E Writing Standards — 5-Point Checklist

Every test written in this project must satisfy all five:

1. **Label visibility checks** — assert every field label is visible before interacting with it
2. **Scoped locators** — if a label appears more than once in the DOM, scope the locator to its parent container
3. **Store entered data** — assign every value typed into a form to a variable, then reuse it for assertions
4. **Review page assertions** — on any confirmation or summary screen, assert all stored values are displayed
5. **Step-by-step assertions** — after each significant action, assert the expected outcome before moving on

These are non-negotiable. Apply all five to every test, every session.

---

## DOM Recon — Required Before Writing Code

Before writing any test code, Claude Code must:

1. Navigate to the target page
2. Catalog all form fields, labels, roles, and parent containers
3. Identify any duplicate labels and plan a scoping strategy
4. Map the full user flow, including what appears on review or confirmation screens

*Then* write the code. This eliminates two or three debugging cycles.

---

## Locator Priority Order

Always follow this order. Only move to the next option if the previous is not available:

1. `getByRole()` — semantic, always preferred
2. `getByLabel()` / `getByText()`
3. `locator()` with CSS selector — only as a last resort
4. XPath — avoid unless there is no other option

---

## File Naming Conventions

- Test files: `feature-name.spec.ts`
- Page objects: `FeatureNamePage.ts`
- Fixtures: `fixtures.ts` in the relevant feature folder

---

## What Not To Do

- Do not add `page.waitForTimeout()` — use proper waiting strategies instead
- Do not duplicate locators already defined in a POM class
- Do not skip the DOM recon step to save time
- Do not write tests that depend on execution order
