# Slide deck content integrity

## Sub-features

Headmatter config parses; every slide's `layout:`/`class:` config applies (config after content would render as visible literal text); every image/asset referenced from slides.md exists where Slidev serves it; code fences balance.

## How to get to it (user POV)

Open the deck and page through it: broken asset references show as broken images, misplaced config shows as stray text like "layout: cover" printed on a slide, and an unparseable package.json means the deck cannot even be installed.

## Driving it with the Node helper

Run `node .cursor/skills/verify-cpp-performance-deck/helpers/drive.mjs` from the repository root. It checks `./package.json`, `./slides.md`, all local references and stylesheets, then writes `artifacts/verification/verify-cpp-performance-deck.json`. A pass is exit code 0 with `"passed": true`.

## Gotchas

- A pass proves source integrity only; it is not a rendered-pixels check. After dependency or Slidev config changes, also run the real build in [Build and export](build-and-export.md).
- Unreferenced files under `./public/` are warnings, not failures.
- The expected deck structure is part of this map: do not silence a structure failure by relaxing the helper - fix the deck.
- Two meme PNGs under `public/memes/` are currently unreferenced (warnings in evidence); that is dead weight, not a failure - do not delete content to silence it.
