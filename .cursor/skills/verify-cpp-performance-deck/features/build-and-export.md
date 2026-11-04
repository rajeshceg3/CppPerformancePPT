# Build and export

## Sub-features

`npm run build` compiles the deck to a static site in `./dist/`; `npm run export` renders a PDF.

## How to get to it (user POV)

`cd . && npm install && npm run build`; open `dist/index.html` or host `dist/` on any static host.

## Driving it with the Node helper

Run the helper first for the fast structural pass, then run the real build above - it is the extended proof for this feature and must exit 0 with a populated `dist/`.

## Gotchas

- PDF export needs a Playwright Chromium download;`playwright-chromium` is a devDependency for PDF export - use `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1 npm install` when you only need dev/build.
- `dist/` and `node_modules/` are gitignored build output, not sources.
- Two meme PNGs under `public/memes/` are currently unreferenced (warnings in evidence); that is dead weight, not a failure - do not delete content to silence it.
