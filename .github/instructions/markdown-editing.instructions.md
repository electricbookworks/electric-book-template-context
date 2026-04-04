---
description: "Use when editing book content markdown files: chapters, covers, title pages, contents pages, and other book content. Covers frontmatter conventions, include syntax, and file naming."
applyTo: "**/*.md"
---

# Book Content Editing

This project is built on the Electric Book template (EBT). These conventions apply to book content files — markdown files inside book folders.

To identify book folders, check `_data/works/` — each subfolder corresponds to a book directory at the project root.

## Frontmatter

Every content file must have YAML frontmatter with at least `title`.

```yaml
---
title: "Chapter Title"
---
```

Every content file has a `style`. If one isn't defined in the YAML frontmatter, it defaults to `default-page`.

Common `style` values (space-separated CSS classes applied to `<body>`):
- `cover-page` — book cover
- `half-title-page` — half title
- `title-page` — full title page
- `copyright-page` — copyright/imprint
- `contents-page` — table of contents
- `default-page` — standard chapter/content page
- `chapter-opener` — chapter opener with special styling

Optional frontmatter keys:
- `notes: footnotes` — override default endnote behaviour for this page
- `layout` — override the default layout (rarely needed)

Check existing files in the project for any project-specific `style` values or frontmatter keys.

## File naming

Files use zero-padded numeric prefixes for reading order. Alphabetical sort = reading order:

```
0-0-cover.md          ← front matter (cover)
0-1-titlepage.md      ← front matter (title)
0-2-copyright.md      ← front matter (copyright)
0-3-contents.md       ← front matter (contents)
01.md                 ← chapter 1
02.md                 ← chapter 2
```

Naming conventions may vary across projects. Match the existing pattern in the book folder.

## Include tags

### Figures

```liquid
{% include figure
   image="filename.jpg"
   alt="Description of the image"
   caption="**Figure 1.** Caption with *markdown* support."
   reference="Figure 1"
%}
```

Additional parameters: `images` (comma-separated), `alt-text` (pipe-separated for multiple), `source`, `class`, `link`, `html`, `markdown`, `image-height`.

### Images (simple)

```liquid
{% include image file="photo.jpg" alt="Description" %}
```

Additional parameters: `class`, `id`, `folder`, `position`, `loading`.

### Video

```liquid
{% include video id="dQw4w9WgXcQ" host="youtube" %}
{% include youtube id="dQw4w9WgXcQ" %}
{% include vimeo id="123456789" %}
```

Additional parameters: `image`, `start`, `subtitles`, `description`, `class`.

### Table of contents

```liquid
{% include toc %}
```

### Questions (interactive MCQ)

```liquid
{% include question file="questions/question-1.md" %}
```

> Not all includes are available in every EBT project. Check `_includes/` to see what's available.

## Do not

- Add `permalink` to frontmatter — EBT uses `permalink: none` globally
- Edit images in format subdirectories — edit `_source/` originals instead
- Skip the `title` or `style` frontmatter keys
