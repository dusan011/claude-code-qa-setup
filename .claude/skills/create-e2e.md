# Skill: create-e2e

Use this skill when asked to create a new end-to-end test.

## Overview

Creates a Playwright E2E test from a ticket or requirement.
Follows a strict 5-step workflow — no skipping steps.

---

## Workflow

### Step 1 — Gather Requirements

Before writing any code:
- Ask for the ticket/test ID
- Fetch the acceptance criteria from the ticket
- Clarify any ambiguous requirements before proceeding
- Identify which user role the test runs as

Do not write any code until requirements are confirmed.

---

### Step 2 — DOM Recon

Navigate to the target page in the browser and:
- Catalog all form fields, labels, roles, and parent containers
- Identify any labels that appear more than once in the DOM
- Plan a scoping strategy for duplicate labels (identify parent containers)
- Map the full user flow, including what appears on review or confirmation screens
- Note the exact text of all labels, buttons, and headings

Document the recon findings before writing code.

---

### Step 3 — Write the Test

Using the POM pattern:
- Create or extend the relevant Page Object class
- Write the test using the 5-point checklist:
  1. Assert every field label is visible before interacting
  2. Scope locators to parent containers where labels are duplicated
  3. Store every entered value in a variable
  4. Assert all stored values on any review or confirmation screen
  5. Assert the expected outcome after each significant action

---

### Step 4 — Run and Validate

- Run only the new test: `npx playwright test path/to/test.spec.ts`
- If it fails, diagnose and fix before presenting
- Do not present a failing test

---

### Step 5 — Self-Check

Before showing the result, verify against the checklist:
- [ ] All 5 checklist items are present
- [ ] No programmatic login calls inside the test
- [ ] All locators follow the priority order (role → label/text → CSS)
- [ ] No duplicate locators that already exist in a POM
- [ ] Test passes locally

Only present the test after this self-check passes.
