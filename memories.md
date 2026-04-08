# EBT memories

Accumulated learnings from working on Electric Book Template projects.
Updated by the agent over time; committed manually to dotfiles.

<!-- Agent: append new learnings below with a date prefix, e.g. "2026-03-29: ..." -->

2026-04-08: Built `eb refine` command (electric-book-modules issue #162)

- **What**: Automated detection and fixing of widows, orphans, and short lines in PDF output. Adds `{:.tighten-N}` IAL classes to source markdown based on Prince's layout analysis.
- **Architecture**: Hybrid — Prince-side ES5 script (`prince-refine.prince`) detects issues using Box Tracking API + `registerPostLayoutFunc`, applies tighten classes to DOM, logs structured manifest to stdout. Node orchestrator (`refine/index.js`) parses manifest, fingerprint-matches elements to source markdown paragraphs, writes IALs.
- **Key files** (all in `electric-book-modules/_tools/run/`):
  - `commands/refine.js` — yargs entry point (`--refine-method`, `--dry-run`)
  - `helpers/lib/refine/index.js` — orchestrator
  - `helpers/lib/refine/prince-refine.prince` — Prince-side detection (ES5)
  - `helpers/lib/refine/injectScript.js` — injects/removes script in merged HTML
  - `helpers/lib/refine/parseManifest.js` — parses `REFINE_CHANGE|...` lines
  - `helpers/lib/refine/mapToSource.js` — fingerprint + fuzzy text matching
  - `helpers/lib/refine/applyClasses.js` — writes IALs (handles preceding IALs too)
  - `helpers/lib/refine/parsePdf.js` + `detectIssues.js` — pdfjs-dist fallback
  - `helpers/lib/runPrince.js` — extended with `options` param (`onStdout`, `maxPasses`)
- **Fingerprinting**: `PARENT_TAG-SIBLING_IDX-ELEMENT_TAG-SIBLING_IDX-openingslug-closingslug`. Computed identically in Prince (ES5) and Node. Achieves ~70% mapping rate on samples book (45/63).
- **Prince constraints**: Prince for Books 20220701 (Prince 14). ES5 only, no strict mode. No `parentElement` (DOM Level 4) — must use `parentNode`. The `.prince` file extension prevents Node's `require-all` from auto-loading it.
- **Severity scale** (from issue #162, higher = worse): 1=short-line, 2=widow(verso), 3=widow(recto), 4=wide-orphan(recto), 5=wide-orphan(verso), 6=narrow-orphan(recto), 7=narrow-orphan(verso).
- **`runPrince.js` change**: Added optional second arg `{ onStdout, maxPasses }`. Backward-compatible — existing callers unaffected. Refine uses `maxPasses: 5` (default is 3).
- **Prerequisite**: `eb output` must be run first to generate `_site/<book>/merged.html`. Refine injects its script, runs Prince, then removes the script.
- **Docs**: `electric-book/_docs/layout/page-refinement.md`
