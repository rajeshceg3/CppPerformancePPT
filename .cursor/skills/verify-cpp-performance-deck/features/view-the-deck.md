# View the presentation locally

## Sub-features

Dev server with hot reload; slide navigation; theme and custom styles applied; images render.

## How to get to it (user POV)

`cd . && npm install && npm run dev`, then open http://localhost:3030 and page through with Space.

## Driving it with the Node helper

The static helper proves the sources the dev server will consume (config placement, asset paths) are sound before you spend an install. It does not replace watching the rendered deck; for visual verification, run the dev server and look.

## Gotchas

- npm installs several hundred MB; `playwright-chromium` is a devDependency for PDF export - use `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1 npm install` when you only need dev/build.
- Port 3030 must be free. Stop the server with Ctrl-C when done.
- Two meme PNGs under `public/memes/` are currently unreferenced (warnings in evidence); that is dead weight, not a failure - do not delete content to silence it.
