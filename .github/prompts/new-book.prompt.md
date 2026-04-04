---
description: "Scaffold a complete new book folder with all required files: cover, title page, copyright, contents, first chapter, styles, image directories, and metadata."
agent: "agent"
argument-hint: "Book title and optional details (author, language, description)"
---

Create a new book in this Electric Book template project.

Before starting, examine the existing project to understand its conventions:
- Check `_data/works/` to see how existing books are set up
- Look at an existing book folder for file naming patterns and frontmatter conventions
- Check an existing `{book}/styles/web.scss` for the SCSS structure
- Check `_data/works/{existing-book}/default.yml` for the metadata structure

## Steps

1. **Choose a folder name**: Use the book title as a lowercase, hyphenated folder name (e.g. `my-new-book`). Create it at the project root.

2. **Create content files** following the naming pattern used by existing books in this project. At minimum:

   - Cover page — style: `cover-page`
   - Title page — style: `title-page`
   - Copyright page — style: `copyright-page`
   - Contents page — style: `contents-page` (include `{% include toc %}`)
   - First chapter — style: `default-page`
   - `index.md` — book landing page

   Every file needs `title` and `style` in the frontmatter.

3. **Create image directories**:
   - `{book}/images/_source/`
   - `{book}/images/web/`
   - `{book}/images/print-pdf/`
   - `{book}/images/screen-pdf/`
   - `{book}/images/epub/`
   - `{book}/images/app/`

4. **Create style entry points** (each needs `---`/`---` YAML front matter). Follow the pattern from existing book styles:
   - `{book}/styles/web.scss`
   - `{book}/styles/print-pdf.scss`
   - `{book}/styles/screen-pdf.scss`
   - `{book}/styles/epub.scss`
   - `{book}/styles/app.scss`

   Each should import its template master (e.g. `@import 'template/web';`).

5. **Create metadata** at `_data/works/{book}/default.yml`. Follow the structure of existing metadata files in `_data/works/`.

6. **Create EPUB support files** if existing books have them:
   - `{book}/package.opf`
   - `{book}/toc.ncx`

7. **Summary**: List all created files and explain what to do next (edit content, add images, run `npm run eb -- output -b {book}`).
