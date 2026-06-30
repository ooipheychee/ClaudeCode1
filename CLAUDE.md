# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Nokia Snake II — a single-file browser game (`snake.html`) styled as a retro Nokia phone UI. No build step, no dependencies, no package manager. Open the file directly in a browser to run it.

## Architecture

Everything lives in one file with three sections:

**CSS** (lines 7–140): Styles the Nokia phone shell, the green LCD screen, D-pad, soft keys, and share modal. Responsive via `min()` and `vw` units; touch-friendly via `touch-action` and `-webkit-tap-highlight-color`.

**HTML** (lines 142–225): Static phone shell structure. The game renders onto `<canvas id="gameCanvas">`. HUD elements (`scoreEl`, `levelEl`, `hiEl`, `levelFill`) are updated by JS.

**JavaScript** (lines 227–529): Self-contained game engine with no frameworks.
- **Canvas sizing**: `calcCell()` computes cell pixel size responsively; called once on load.
- **Audio**: Web Audio API, lazy-initialized (`ac()`). Four sound functions: `sndEat`, `sndDie`, `sndLevel`, `sndStart`.
- **Game state**: `snake` (array of `{x,y}`), `facing`/`wantDir` (direction indices 0–3 = down/up/left/right), `food`, `score`, `level`, `ate`, `phase` (`start|playing|paused|over`).
- **Loop**: `setInterval`-based via `loop()` / `tick()`. Speed is looked up from the `SPD` array (ms per tick) indexed by level. `dirLocked` prevents multiple direction changes per tick.
- **Progression**: 4 foods eaten = level up; 20 levels total. Score = `level × 10` per food. High score persisted in `localStorage` key `snk_hi`.
- **Input**: Keyboard (`keydown`), touch swipe on canvas, and D-pad button clicks all call `dir2(d)`.
- **Draw**: `draw()` renders background grid, food, body, head with directional eyes, and phase-specific overlays (start/paused/game-over). A `requestAnimationFrame` loop runs always to animate the blinking food on game-over.
- **Share**: Modal with two states — hosted (shows WhatsApp share link) vs local file (shows Netlify drag-and-drop instructions).

## Git & GitHub

- Repo: https://github.com/ooipheychee/ClaudeCode1
- After every meaningful change: stage specific files, commit with a clean message, and push to `origin main`.
- Auth: HTTPS via GitHub CLI credential helper (already configured).
