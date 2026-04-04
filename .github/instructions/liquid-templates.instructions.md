---
description: "Use when editing Jekyll Liquid templates: layouts, includes, conditional rendering by output format, metadata loading, and format-specific logic."
applyTo: "_includes/**,_layouts/**"
---

# Liquid Template Development

This project is built on the Electric Book template (EBT).

## Output format conditionals

The `site.output` variable controls format-specific rendering. Values: `web`, `print-pdf`, `screen-pdf`, `epub`, `app`.

Common pattern — PDF and EPUB vs web/app:

```liquid
{% if site.output == 'epub' or site.output == 'print-pdf' or site.output == 'screen-pdf' %}
  {# Markup for print and EPUB #}
{% else %}
  {# Markup for web/app #}
{% endif %}
```

Always consider all five output formats when adding conditional logic. Test that your changes don't break any format.

## Metadata loading

Includes that need book metadata **must** call `{% include metadata %}` before accessing metadata variables. This sets up variables like `book-directory`, `book-subdirectory`, `is-translation`, `language`, `build` (development/live), and loads the correct `_data/works/` YAML.

```liquid
{% include metadata %}
{{ title }}
{{ book-directory }}
```

## Layout hierarchy

Typical EBT layout hierarchy (may vary by project):

- `compress.html` — HTML minification wrapper
- `default.html` — main layout (uses `layout: compress`), branches on `site.output`
- `min.html` — bare content, no compression
- `opf.html` / `ncx.html` — EPUB packaging layouts
- `trim.html` — content only, no markup

## Include conventions

- Includes are parametric — document expected parameters in a comment at the top if adding new ones
- Use `include.parameter-name` to access parameters passed by the caller
- Check the project's `_includes/figure` and `_includes/image` for examples of well-structured parametric includes

## Key variables available in templates

From `{% include metadata %}`:
- `book-directory` — the book's folder name
- `book-subdirectory` — translation subfolder (if any)
- `is-translation` — boolean
- `language` — current language code
- `build` — `"development"` or `"live"`

From `_data/settings.yml` (accessed via `site.data.settings`):
- `site.data.settings.web.pagination`
- `site.data.settings.web.annotator`
- `site.data.settings.web.svg.inject`
- See `_data/settings.yml` for the full list

Available settings vary by EBT version. Check `_data/settings.yml` in the project.

## Do not

- Use `site.output` values other than `web`, `print-pdf`, `screen-pdf`, `epub`, `app`
- Access book metadata without first calling `{% include metadata %}`
- Add custom Jekyll plugins — EBT projects typically use `github-pages` gem for compatibility
- Assume web-only features work in PDF/EPUB (e.g. JavaScript interactivity, lazy loading)
