---
description: "Use when editing Sass/SCSS stylesheets: customizing book styles, overriding variables, adding CSS rules, working with the Sass cascade."
applyTo: "**/*.scss"
---

# Sass Styling

This project is built on the Electric Book template (EBT).

## YAML front matter requirement

Any `.scss` file that Jekyll processes **must** start with empty YAML front matter:

```scss
---
---

// Your SCSS here
```

Without this, Jekyll won't compile the file.

## Sass cascade

Modern EBT projects have a four-layer cascade (older projects may have fewer layers):

1. `_sass/template/` — core defaults from `electric-book-modules` (do not edit)
2. `_sass/base/` — optional design-system layer
3. `_sass/custom/` — project-level customizations (primary editing target)
4. `{book}/styles/` — per-book overrides (e.g. `mybook/styles/web.scss`)

Check `_sass/` to see which layers exist in this project.

## Where to edit

- **Project-wide changes**: edit files in `_sass/custom/` (e.g. `_sass/custom/web-rules.scss`)
- **Single-book changes**: edit `{book}/styles/{format}.scss`
- **Never edit** `_sass/template/` — those files come from the npm modules package

## Key variables

Override these in `_sass/custom/` or `{book}/styles/` (without `!default`):

```scss
// Typography
$font-text-main: "Georgia", serif;
$font-display-main: "Helvetica Neue", sans-serif;
$font-size-default: 1rem;
$line-height-default: 1.5;

// Layout
$max-width-default: 45rem;

// Spacing
$spaced-paras: false;  // true = space between paragraphs, false = indented first line
```

Color variables are typically in `_sass/template/colors.scss` and `_sass/custom/colors.scss`.

## Format-specific entry points

Each format has its own SCSS entry point. The `$output-format` variable is set automatically:

| Format | Entry point | `$output-format` |
|---|---|---|
| Web | `web.scss` | `"web"` |
| Print PDF | `print-pdf.scss` | `"print-pdf"` |
| Screen PDF | `screen-pdf.scss` | `"screen-pdf"` |
| EPUB | `epub.scss` | `"epub"` |
| App | `app.scss` | `"app"` |

## Book-level SCSS structure

```scss
---
# Web styles
---

// Set variables (without !default to override template defaults)
$font-text-main: "My Custom Font", serif;

// Import fonts
@font-face { /* ... */ }

// Import template master styles
@import 'template/web';

// Add custom CSS below the import
.my-custom-class {
  color: red;
}
```

## Do not

- Edit files in `_sass/template/` — run `npm run update-modules` to update those
- Forget the `---` / `---` front matter on Jekyll-processed SCSS files
- Use `!default` when overriding variables (that would preserve the template default)
