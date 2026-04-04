# Electric Book Template — AI Context

Copilot customization files for projects built on the [Electric Book template](https://github.com/electricbookworks/electric-book) (EBT).

## What's here

| File | Type | Purpose |
|---|---|---|
| `.github/copilot-instructions.md` | Workspace instructions | Always-on project guidelines: architecture, commands, conventions, pitfalls |
| `.github/agents/book-producer.agent.md` | Custom agent | Non-technical book production: editing content, adding figures, managing metadata |
| `.github/instructions/markdown-editing.instructions.md` | File instructions | Auto-activates when editing `**/*.md` in book folders |
| `.github/instructions/sass-styles.instructions.md` | File instructions | Auto-activates when editing `**/*.scss` |
| `.github/instructions/liquid-templates.instructions.md` | File instructions | Auto-activates when editing `_includes/**` or `_layouts/**` |
| `.github/prompts/new-book.prompt.md` | Prompt | `/new-book` slash command to scaffold a complete book folder |

## Setup

Copy or symlink the `.github/` folder into any EBT-based project:

```sh
# Option 1: symlink (keeps in sync with this repo)
ln -s /path/to/context-ebt/.github /path/to/your-project/.github

# Option 2: copy (snapshot, won't auto-update)
cp -r /path/to/context-ebt/.github /path/to/your-project/
```

If the project already has a `.github/` folder (e.g. with workflows), symlink or copy the individual subdirectories instead:

```sh
ln -s /path/to/context-ebt/.github/copilot-instructions.md /path/to/your-project/.github/
ln -s /path/to/context-ebt/.github/agents /path/to/your-project/.github/
ln -s /path/to/context-ebt/.github/instructions /path/to/your-project/.github/
ln -s /path/to/context-ebt/.github/prompts /path/to/your-project/.github/
```

## Cross-project compatibility

These files are written to work across EBT-based projects of varying ages and customisation levels:

- Book folder names are not hardcoded — instructions tell the agent to discover them from `_data/works/`
- The `applyTo` patterns use broad globs (`**/*.md`, `**/*.scss`) rather than specific folder names
- Notes flag areas where older EBT versions may differ (e.g. command syntax, Sass structure)
- The agent is instructed to check existing project files for patterns before making changes

## Customising per project

If a specific project needs additional instructions, add a project-level `.github/instructions/` file with a narrow `applyTo` pattern. It will complement (not replace) the files from this repo.
