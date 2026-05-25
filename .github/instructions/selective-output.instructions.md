---
description: "Use when a user asks to build/output/render only a specific unit, chapter, or section of a book in a specific format (print-pdf, screen-pdf, epub). Covers the two-file pattern for limiting builds to a subset of content."
applyTo: "_configs/**,_data/works/**"
---

# Selective output: building only specific units

When asked to output only certain units/chapters of a book (optionally in a
specific language), two files must be modified:

## 1. Format config: `_configs/_config.{format}.yml`

In the `exclude:` list:

- **Uncomment** excludes for entire volumes not needed (e.g. `- microeconomics`).
- **Uncomment** excludes for languages not needed (e.g. `- macroeconomics/ko`).
- **Uncomment** chapter-glob excludes for all chapters EXCEPT the target
  (e.g. uncomment `- macroeconomics/01*` through `- macroeconomics/08*` and
  `- macroeconomics/10*`, leaving `# - macroeconomics/09*` commented).
- **Uncomment** front-matter excludes (`0-0-cover*`, `0-1-titlepage*`, etc.)
  unless the user wants them included.
- Leave the target chapter's glob **commented** (i.e. not excluded).

## 2. Book metadata: `_data/works/{book}/{lang}/default.yml`

In the target format's `files:` list (e.g. under `screen-pdf:` → `files:`):

- **Comment out** all file entries except those belonging to the target unit.
- The target unit's files stay uncommented.

## Conventions

- Use `# - ` prefix to comment out YAML list items.
- Do not delete lines — only toggle comments, so the change is easily reversible.
- If the user says "only unit 9", that means files prefixed `09-*`.
- If no language is specified, ask which language.
- If no format is specified, ask which format.
- After making changes, remind the user to revert these files before committing
  (these are local build-speed tweaks, not permanent changes).
