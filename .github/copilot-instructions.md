# Electric Book Template — Project Guidelines

This project is built on the [Electric Book template](https://github.com/electricbookworks/electric-book) (EBT), a Jekyll-based multi-format book-production system. It outputs books as websites, print PDFs, screen PDFs, EPUBs, app files, and sometimes MS Word documents.

**When you first respond in a session, briefly confirm that you have loaded the Electric Book template context.** A single sentence is enough, e.g. "EBT context loaded." This lets the user know you have project-specific guidance available.

**Two audiences** use EBT projects:
- **Non-technical users** produce books: editing markdown, setting metadata, managing images, and running output commands.
- **Technical users** extend the template: modifying layouts, includes, Sass, JavaScript, Gulp pipelines, and the `electric-book-modules` npm package.

> **Note:** The EBT has evolved over the years. Older projects may not have all features described here, and some may use slightly different conventions or folder structures. Always check the actual project files before assuming a feature exists.

## Quick Reference

| Task | Command |
|---|---|
| First-time setup | `npm run setup` |
| List all commands | `npm run eb` |
| Serve website locally | `npm run eb -- output` |
| Build print PDF | `npm run eb -- output -f print-pdf` |
| Build screen PDF | `npm run eb -- output -f screen-pdf` |
| Build EPUB | `npm run eb -- output -f epub` |
| Build app files | `npm run eb -- output -f app` |
| Specific book + language | `npm run eb -- output -f print-pdf -b mybook -l fr` |
| Process images | `npm run eb -- images` |
| Rebuild search indexes | `npm run eb -- reindex` |
| Update modules package | `npm run update-modules` |

Older EBT projects may use different commands (e.g. `gulp`, `bundle exec jekyll serve`). Check `package.json` for available scripts.

There is no automated test suite (`npm test` is not implemented).

## Architecture

### Output format system

Each format has a config overlay in `_configs/` merged with `_config.yml` at build time. The `site.output` Liquid variable (`web`, `print-pdf`, `screen-pdf`, `epub`, `app`) controls conditional rendering throughout includes and layouts. Conditional logic like `{% if site.output == 'epub' %}` is pervasive — respect it when editing templates.

### Book structure

Books live in top-level folders. Each project chooses its own folder names (e.g. `book/`, `my-novel/`, `textbook/`). Discover which books exist by checking `_data/works/` — each subfolder there corresponds to a book folder.

Each book folder typically contains:
- Numbered markdown files (`0-0-cover.md`, `01.md`) — alphabetical sort = reading order
- `images/` with subdirectories per format (`_source/`, `web/`, `print-pdf/`, `epub/`, etc.)
- `styles/` with one SCSS entry point per format (`web.scss`, `print-pdf.scss`, etc.)

**Frontmatter conventions**: every content file needs `title` and `style` (CSS classes for `<body>`, e.g. `cover-page`, `contents-page`, `default-page`).

### Metadata and data files

- `_data/works/{book}/default.yml` — full book metadata (title, creator, products, identifiers)
- `_data/works/{book}/{lang}/default.yml` — translation overrides
- `_data/settings.yml` — project-wide feature toggles
- `_data/project.yml` — publisher/organisation identity
- `_data/locales.yml` — UI strings keyed by ISO 639-1 language code

Includes that need book metadata must call `{% include metadata %}` first.

### Translations

Translations are subfolders inside book folders named by language code (e.g. `mybook/fr/`). They inherit parent images, styles, and metadata unless overridden. Project-level translated pages go in top-level language folders (e.g. `fr/index.md`).

### Sass cascade

Four layers, each overriding the previous:
1. `_sass/template/` — core defaults (from `electric-book-modules` npm package; do not edit)
2. `_sass/base/` — optional design-system layer
3. `_sass/custom/` — project-level customizations (main editing target)
4. `{book}/styles/` — per-book overrides

Any `.scss` file processed by Jekyll must have empty YAML front matter (`---` / `---`) at the top.

Older EBT projects may have a simpler Sass structure without the `base/` or `custom/` layers.

### JavaScript

- Entry point: `assets/js/main.js` — imports modules from `@electricbookworks/electric-book-modules`
- Bundled by webpack (`_webpack/webpack.config.js`) into `assets/js/dist/`
- For PDF output, transpiled to ES5 (PrinceXML compatibility — no arrow functions, no `const`, no modules)
- Follows **Standard JS** syntax (no semicolons)

### Includes

Reusable components are called from markdown with `{% include figure %}`, `{% include image %}`, `{% include video %}`, `{% include toc %}`, `{% include question %}`, etc. See `_includes/` for the full set. Many are parametric — check each include's source for expected parameters.

### External modules

The bulk of JS, Sass, and Gulp processors live in the separate [`electric-book-modules`](https://github.com/electricbookworks/electric-book-modules) npm package. Template-level files are thin wrappers. Run `npm run update-modules` after changing the version in `package.json`.

Older projects may not use `electric-book-modules` — they may have all code inline.

## Conventions

- **`permalink: none`** in `_config.yml` — URLs match folder/filename exactly. Do not add `permalink` to frontmatter; predictable file paths are required for PDF/EPUB packaging.
- **Numbered filenames** — book files use zero-padded prefixes for reading order (`0-0-cover.md`, `0-3-contents.md`, `01.md`, `02.md`).
- **`style:` frontmatter** — this project-specific key sets CSS classes on `<body>`, controlling per-page layout. It is not standard Jekyll.
- **Image sets** — never edit images in format subdirectories directly. Edit `_source/` originals and run `npm run eb -- images` to regenerate.
- **No custom Jekyll plugins** — the project uses `github-pages` gem for GitHub Pages compatibility.
- **Prefer Javascript for scripting** — When proposing new scripts, prefer Javascript over Python, Ruby, or other languages.
- **AI-generated scripts** — Save AI-generated tooling in `_tools-custom/ai/[verb]/[subject]`. These scripts must follow the project's ESLint config (`.eslintrc.json`). AI-generated scripts are sandboxed and not expected to be reviewed or maintained by the dev team. See the custom-scripts instruction file for full details.

## Documentation

If this project includes `_docs/`, comprehensive documentation is available there (and served at the project website). Key references:

- Setup: `_docs/setup/` (quick-start, metadata, translations, variants)
- Editing: `_docs/editing/` (markdown, images, figures, tables, notes, questions, video)
- Output: `_docs/output/` (web, PDF, EPUB, app, Word)
- Advanced: `_docs/advanced/` (API, HTML transformations, JS, packaging)
- Troubleshooting: `_docs/troubleshooting/`

The canonical template docs are at https://electricbookworks.github.io/electric-book/docs/

## Pitfalls

- **Missing SCSS front matter**: any `.scss` file Jekyll must process needs `---\n---` at the top or it won't be compiled.
- **Stale modules**: `npm install` may not update `electric-book-modules` due to its GitHub-tag versioning. Always use `npm run update-modules`.
- **Image format mismatch**: if images aren't appearing, check that the correct format subdirectory (`web/`, `print-pdf/`, etc.) contains the file and that `image-set` in the config matches.
- **PDF JavaScript**: PrinceXML requires ES5. The webpack config handles this, but any new JS must be compatible with Babel transpilation.
- **EPUB validation**: EPUB output goes through extensive post-processing (Gulp tasks for XHTML conversion, link rewriting, file cleanup). Test with an EPUB validator after changes to includes or layouts.
