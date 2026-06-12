# Tiles Puzzle Game

A modern JavaScript implementation of a configurable tiles puzzle game (8-, 15-,
or 24-puzzle) with dynamic image-based tilemap support. Select an image and
solve the puzzle by rearranging tiles to reconstruct the original image.

## Key Features

✨ **Dynamic Image Selection**: Load JPEG, PNG, WebP, or GIF images (up to 12
MB) to create a custom puzzle

🔒 **Privacy-First Processing**: Images are processed locally in your browser
only

🎨 **Automatic Resolution Detection**: The game automatically detects image
dimensions and scales accordingly

🧩 **Canvas-Based Tile Extraction**: Each puzzle tile is intelligently
extracted from the original image using the HTML5 Canvas API

🕳️ **Intelligent Gap Management**: The bottom-right tile is automatically
removed to create the empty space

🔀 **Fair Shuffling**: 200 random valid moves with minimum board-distance
checks to avoid near-solved starts

⌨️ **Flexible Controls**: Click/touch, Arrow keys, WASD, Shift+Arrow or
Shift+WASD for whole-line moves, `R` to reshuffle, and `?` for shortcut help

❓ **Shortcut Help**: Press `?` any time to show available keyboard shortcuts
in the status area

✅ **Move Validation**: Click any tile aligned with the gap (same row/column)
to shift the entire line toward the gap; single-step adjacent moves still work

🏆 **Win Detection**: The game automatically detects when you've successfully
solved the puzzle

🎊 **Win Celebration**: Enjoy a 5-second full-screen particle animation
(fireworks and confetti) when you complete the puzzle!

📱 **Viewport-Adaptive Layout**: Board, controls, and menu rows adapt to
narrow/mobile viewports to avoid horizontal overflow and cropping

🖥️ **Dynamic Celebration Canvas**: Celebration rendering uses the effective
visual viewport and scales with browser UI changes (mobile address bars,
rotation, resize)

🖼️ **Solved Overlay**: On solving the puzzle the full image fades in while
the tiles fade out; reshuffling reverses the effect

📦 **Installable PWA**: Add Tiles Puzzle to your device from the in-app
**Install App** action when the browser surfaces the install prompt

📡 **Offline-First Asset Caching**: App shell and static assets are cached by
the service worker; navigation and assets continue to work offline after first
load

📊 **Move Counter**: Track your performance with a built-in move counter,
best-today, and all-time best per difficulty

🎯 **Three Difficulty Levels**: 3×3 (8-Puzzle), 4×4 (15-Puzzle), 5×5
(24-Puzzle) — switchable any time

🧪 **Seeded Shuffle Mode**: Use the built-in seed input (or `?seed=your-seed`
URL) for deterministic board shuffles during debugging; use **Clear** to return
to non-deterministic shuffling

♻️ **Reset Settings**: Restore defaults for grid size, number overlay, and
saved scores with one action

## Privacy & Data Handling

- Your selected image stays on your device and is processed locally in the
  browser.
- Images are not uploaded to a server.
- Images are not collected, stored, or shared by this project.
- Processing uses browser APIs (`FileReader` and `Canvas`) in your local
  session only.

## Usage

### Getting Started

1. Serve `javascript/html5/src/` through HTTP(S) (required for service workers
   and full PWA):

```bash
npx http-server javascript/html5/src -p 4173 -c-1
```

1. Open `http://127.0.0.1:4173/index.html` in a web browser
1. Click the file input to select a JPEG, PNG, WebP, or GIF image (max 12 MB)
1. Choose a difficulty: **3×3**, **4×4** (default), or **5×5**
1. Click on tiles adjacent to the empty space to move them
1. Reconstruct the original image to win
1. Click "Shuffle Again" (or press `R`) to start a new puzzle

> Note: Opening the app via `file://` disables service workers in most
> browsers, so install/offline support will not be active.

## PWA Support

### Install Flow

- The app listens for `beforeinstallprompt` and reveals **Install App** in the
  side menu when available.
- Accepting the install prompt allows launching Tiles Puzzle as a standalone
  app.
- After successful installation (`appinstalled`), the install action is hidden
  automatically.

### Offline Caching Strategy

- **Precache (install-time)**: Core app shell and static assets (HTML, CSS,
  JS modules, manifests, icons, default images).
- **Navigation requests**: Network-first with cached fallback to keep pages
  available offline.
- **Asset requests (same-origin GET)**: Cache-first for fast repeat loads and
  offline resilience.
- **Versioned caches**: Old cache buckets are cleaned up on service worker
  activation.

### Scope and Limits

- PWA caching covers application assets served from this origin.
- User-uploaded local files are processed in-memory/browser session and are not
  persisted by the service worker.

### Game Rules

- Click any tile aligned with the empty space (same row or same column) to
  shift all intervening tiles toward the gap
- Adjacent tile clicks still perform single-step moves
- You can also move via Arrow keys or WASD
- Press Shift+Arrow or Shift+WASD to shift an entire aligned row or column
  toward the gap in one move
- Press `R` anytime during an active game to reshuffle
- **Optional Seeded Shuffle**: Set a value in the **Seed** input field and
  click **Apply** to generate deterministic puzzles (useful for debugging or
  sharing reproducible puzzle states); click **Clear** to remove the seed and
  return to random shuffles
- The goal is to arrange all tiles to recreate the original image
- The puzzle is won when all tiles are in their correct positions
- Celebrate with a 5-second burst of fireworks and confetti particles when you
  win!

### Seeded Shuffle (Debugging & Sharing)

For reproducible puzzles during development or testing:

1. **In-App Control**: Enter a seed value in the **Seed** input field and click
   **Apply** — the puzzle will immediately reshuffle with that seed
2. **URL Parameter**: Alternatively, add `?seed=your-seed` to the URL to load a
   specific seed (e.g., `index.html?seed=12345`)
3. **Clear Seed**: Click the **Clear** button to remove the seed and return to
   random shuffles
4. **URL Sync**: Applying or clearing a seed updates the URL's query parameter
   via `replaceState` for easy sharing

When a seed is set, the shuffle algorithm becomes deterministic — the same
seed always produces the same board layout, allowing you to reproduce puzzle
states for debugging or sharing with collaborators.

## Software Architecture

See [javascript/html5/src/doc/software_architecture.md](javascript/html5/src/doc/software_architecture.md) for the full
software architecture documentation, including component diagrams, state
management, and celebration animation system details.

### Module Map

- [javascript/html5/src/js/tiles_puzzle.js](javascript/html5/src/js/tiles_puzzle.js) — Main `PuzzleGame` controller:
  game state, lifecycle, celebration animation, and seeded shuffle orchestration
- [javascript/html5/src/js/board.js](javascript/html5/src/js/board.js) — Board sizing, adjacency, valid-move,
  puzzle-naming, and seeded RNG helpers
- [javascript/html5/src/js/input.js](javascript/html5/src/js/input.js) — Keyboard/touch gesture mapping and shortcut
  detection
- [javascript/html5/src/js/storage.js](javascript/html5/src/js/storage.js) — localStorage persistence with
  per-grid-per-date isolation, legacy migration, and settings reset
- [javascript/html5/src/js/view.js](javascript/html5/src/js/view.js) — Tile and board DOM rendering helpers
- [javascript/html5/src/sw.js](javascript/html5/src/sw.js) — Service worker with install and offline cache strategies
- [javascript/html5/src/doc/software_architecture.md](javascript/html5/src/doc/software_architecture.md) — Detailed
  architecture, module interaction diagram, celebration particle physics, and
  responsive design notes

## Smoke Testing

Playwright smoke tests are defined in `tests/smoke.spec.js` and validate the
core user flows including:

- Image upload and puzzle initialization
- Keyboard controls (arrows, WASD, Shift+Arrow/Shift+WASD for line shift, R for
  reshuffle, ? for help)
- Responsive layout overflow checks across portrait mobile/tablet viewports
  (including narrow-phone width)
- Difficulty switching and settings persistence
- Accessibility attributes (aria-pressed, ARIA live regions)
- Seeded shuffle (URL parameter and in-app seed controls)
- Score isolation per difficulty and date
- Win condition and celebration animation triggers
- Reset functionality

### Command Matrix

| Goal | Standard | Windows explicit Node |
| --- | --- | --- |
| Unit helpers | `npm run test:unit` | `npm run test:unit:node` |
| Smoke tests (headless) | `npm run test:smoke` | `npm run test:smoke:serial:node` |
| Smoke tests (serial) | `npm run test:smoke:serial` | `npm run test:smoke:serial:node` |
| Smoke tests (headed) | `npm run test:smoke:headed` | n/a |
| Smoke tests (UI) | `npm run test:smoke:ui` | n/a |

### Local execution

1. Install dependencies:

```bash
npm install
```

1. Install Playwright browser binaries (first run only):

```bash
npx playwright install chromium
```

1. Run headless smoke tests (default CI-like mode):

```bash
npm run test:smoke
```

1. Optional run modes:

```bash
npm run test:unit
npm run test:unit:node
npm run test:smoke:headed
npm run test:smoke:ui
npm run test:smoke:serial
npm run test:smoke:serial:node
```

### What each command does

- `npm run test:unit`: Runs helper-level module tests (`board.js`, `input.js`,
  `storage.js`) without browser interaction.
- `npm run test:unit:node`: Windows helper for unit tests via explicit Node
  executable when `node` is shadowed.
- `npm run test:smoke`: Runs tests headless against a temporary local static
  server.
- `npm run test:smoke:serial`: Runs the same tests headless with one worker
  (`--workers=1`) for more stable local troubleshooting.
- `npm run test:smoke:serial:node`: Windows helper that runs Playwright via
  `C:\Program Files\nodejs\node.exe` to avoid environments where `node` is
  shadowed.
- `npm run test:smoke:headed`: Runs the same tests with a visible browser
  window.
- `npm run test:smoke:ui`: Opens Playwright UI mode for interactive debugging
  and re-runs.

### CI execution

Smoke tests also run automatically in GitHub Actions via
`.github/workflows/playwright-smoke.yml` on pushes and pull requests.

For CI parity during local troubleshooting, run both:

```bash
npm run test:smoke
npm run test:smoke:serial:node
```

### Recommended local command

If your shell environment shadows `node`/`npx`, prefer this command as the
default local smoke run:

```bash
npm run test:smoke:serial:node
```

### Troubleshooting (Windows)

If your environment shadows `node`/`npx` (for example by HPC tools), run
Playwright through the explicit Node executable:

```powershell
& "C:\Program Files\nodejs\node.exe" .\node_modules\playwright\cli.js install chromium
& "C:\Program Files\nodejs\node.exe" .\node_modules\playwright\cli.js test
```

## Browser Compatibility

Works in all modern browsers that support:

- HTML5 Canvas API
- CSS Grid Layout
- ES6 JavaScript (Classes, Arrow Functions, Async/Await)
- FileReader API

Tested in:

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## License

This project is licensed under the [MIT License](LICENSE).
