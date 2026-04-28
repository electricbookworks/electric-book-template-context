---
description: "Use when creating or editing AI-generated custom scripts in _tools-custom/ai/. Covers file placement, linting requirements, sandboxing rules, and the relationship between _tools-custom and _tools."
applyTo: "_tools-custom/**"
---

# AI-generated custom scripts

Non-technical team members use AI to create scripts that help with book-production tasks. These scripts are sandboxed so that the dev team does not need to review or maintain them.

## File placement

Save all AI-generated scripts in:

```
_tools-custom/ai/[verb]/[subject]/
```

Examples: `_tools-custom/ai/check/broken-links/`, `_tools-custom/ai/convert/docx-to-md/`.

The `[verb]/[subject]` pattern keeps the `ai/` directory organised as more scripts are added.

## Code style

Scripts must follow the project's ESLint config (`.eslintrc.json`), which extends **Standard JS** style. Key rules:

- No semicolons
- 2-space indentation
- Single quotes for strings
- `'use strict'` at file level (`"strict": ["error", "global"]`)

Run `npx eslint <file>` to check before committing.

## How `_tools-custom` works

`_tools-custom/` is designed to be merged with `_tools/` whenever `@electricbookworks/electric-book-modules` is installed (via `npm install`, `npm run update-modules`, or `yalc`). This means custom scripts *can* have interdependencies with tools from the modules package.

However, fully sandboxed scripts in `ai/` may be run directly from `_tools-custom/` without the merge step. This is the expected pattern for non-developer usage.

**Do not** place AI-generated scripts outside of `_tools-custom/ai/` unless a developer has explicitly asked for it.

## Expectations

- AI-generated scripts are not expected to be reviewed or maintained by the dev team.
- If a script needs dev involvement to work, reconsider whether AI generation is the right approach.
- Prefer JavaScript (Node.js) over Python, Ruby, or other languages, consistent with the rest of the project tooling.
