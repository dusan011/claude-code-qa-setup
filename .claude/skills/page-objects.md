# Skill: page-objects

Use this skill when asked to create or update a Page Object Model (POM) class.

## Core Rule

**Check existing POMs first. Extend them. Do not duplicate.**

Before creating anything new:
1. Search for an existing POM that covers the target page or component
2. If it exists — extend it, do not create a parallel class
3. If it does not exist — create a new one following the structure below

---

## Locator Priority Order

Always follow this order:

1. `getByRole()` — semantic, always preferred
2. `getByLabel()` / `getByText()`
3. `locator()` with CSS selector — only as a last resort
4. XPath — avoid unless there is no other option

---

## Class Structure

```typescript
import { Page, Locator } from '@playwright/test';

export class ExamplePage {
  readonly page: Page;

  // Declare all locators as typed properties in the constructor
  readonly someField: Locator;
  readonly submitButton: Locator;

  constructor(page: Page) {
    this.page = page;
    this.someField = page.getByRole('textbox', { name: 'Field label' });
    this.submitButton = page.getByRole('button', { name: 'Submit' });
  }

  // Methods are async and include their own assertions
  async fillSomeField(value: string): Promise<void> {
    await expect(this.someField).toBeVisible();
    await this.someField.fill(value);
  }

  async submit(): Promise<void> {
    await this.submitButton.click();
  }
}
```

---

## Rules

- Constructor initializes all locators as typed properties — no inline locators in methods
- Methods are async and assert visibility before interacting
- One POM per page or significant component
- If a locator already exists in the POM, reuse it — never redefine it in the test file
