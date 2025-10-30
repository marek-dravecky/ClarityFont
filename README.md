# ClarityFont — Vue Port of the Gradio App

This repository contains a small Vue 3 + Vite app that reproduces the behavior of the original Gradio app: a timed reading test that compares reading speed across fonts and recommends the fastest font.

Files created:
- `index.html` — Vite entry HTML
- `package.json` — scripts and dependencies
- `src/main.js` — bootstraps Vue
- `src/App.vue` — top-level view switcher
- `src/components/Home.vue` — front page / start button
- `src/components/TestPage.vue` — timed reading test UI and logic
- `src/assets/styles.css` — global styles and @font-face for OpenDyslexic

Quick start (macOS / zsh):

1. Install dependencies:

```bash
npm install
```

2. Run the dev server:

```bash
npm run dev
```

Open the dev server URL printed by Vite (usually http://localhost:5173).

Notes:
- This project is intentionally minimal. It uses a local reactive state to replicate the Gradio app's test flow.
- The OpenDyslexic font is loaded via CDN using @font-face in `styles.css`.

If you want, I can run `npm install` and start the dev server for you, or add small improvements (persist results, export summary, tests).
