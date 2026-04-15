# Claude Code QA Setup

A minimal, production-ready Claude Code configuration for QA automation projects using Playwright.

Based on the setup described in [How I turned Claude Code into a disciplined QA engineer](https://dusanpetrovic.dev).

## What's included

```
├── CLAUDE.md                        # Project-wide rules, loaded automatically every session
└── .claude/
    └── skills/
        ├── create-e2e.md            # 5-step E2E test creation workflow
        ├── fix-test-loop.md         # Autonomous debugging loop with hard stops
        └── page-objects.md          # POM consistency rules and class structure
```

## How to use

1. Copy `CLAUDE.md` to the root of your Playwright project
2. Copy `.claude/skills/` to your project root
3. Edit `CLAUDE.md` to match your project — update the auth section, file naming conventions, and any project-specific rules
4. Remove skills you don't need, add your own following the same structure

## What each file does

**`CLAUDE.md`** is read automatically at the start of every Claude Code session. It defines:
- Authentication rules (critical — prevents Claude from breaking your auth model)
- The 5-point E2E writing checklist
- DOM recon requirements before writing code
- Locator priority order

**`create-e2e`** is invoked when you ask Claude Code to write a new test. It enforces:
- Requirement gathering before writing anything
- DOM recon before writing code
- The 5-point checklist on every test
- A self-check step before presenting the result

**`fix-test-loop`** is invoked when you ask Claude Code to fix a failing test. It runs an autonomous debugging loop (max 10 iterations) with hard stops for unrecoverable errors.

**`page-objects`** is invoked when you ask Claude Code to create or update a POM class. It enforces checking for existing POMs before creating new ones, and a consistent class structure.

## Adapting to your project

The structure matters more than the specific content. Replace:
- Auth rules with your actual auth model
- File naming conventions with your team's standards
- Checklist items with your own quality gates

## More

- Blog: [dusanpetrovic.dev](https://dusanpetrovic.dev)

Built by a Senior QA Automation Engineer with 7 years of experience designing test infrastructure across web, mobile, and backend — Playwright, Selenium, Cypress, Appium, Detox, and API automation. I've built automation frameworks from scratch, set testing standards for teams, and integrated test suites into CI/CD pipelines across multiple organizations.