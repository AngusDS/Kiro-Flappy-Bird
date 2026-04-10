# Tech Stack

## Core Technologies
- **Language**: Vanilla JavaScript (ES6+) — no transpilation, no bundler
- **Rendering**: HTML Canvas 2D API (`CanvasRenderingContext2D`)
- **Audio**: HTML `<audio>` elements via the Web Audio API
- **Deployment**: Single `index.html` file — no server required, open directly in browser

## No External Dependencies
There are no npm packages, build tools, frameworks, or CDN imports. Everything runs natively in the browser.

## Common Commands
Since there is no build system, there are no build or test commands. To run the game:
- Open `index.html` directly in a browser, or
- Serve locally with any static file server, e.g.:
  ```
  npx serve .
  python -m http.server 8080
  ```

## Game Loop
Driven by `requestAnimationFrame` for frame-rate-synced updates. Delta time (`dt`) is passed to systems that need time-based logic (e.g., pipe spawn interval).

## Asset Loading
All assets (image + audio) are preloaded via `assetLoader.loadAll()` before the game loop starts. Missing assets are silently ignored — the game continues without them.
