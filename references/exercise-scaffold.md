# Scaffold a New Exercise

Use this whenever a workout's main set needs runnable starter files, not just a written prompt.

## Layout

Create one folder per exercise, named `exercises/YYYY-MM-DD-slug/` (date the workout starts, slug from the fictional scenario, e.g. `2026-07-25-neon-slice`). Each exercise is its own Vite root sharing the practice repo's root `node_modules` and dependencies — do not give an exercise its own `package.json` or install exercise-specific dependencies unless the workout explicitly requires a new library.

```text
exercises/YYYY-MM-DD-slug/
├── EXERCISE.md
├── index.html
└── src/
    ├── main.jsx
    ├── <PascalCaseScenario>App.jsx
    └── styles.css
```

- `EXERCISE.md`: the challenge statement — goal/context, required behavior, accessibility requirements, constraints, the vocabulary rep terms to use when explaining the solution, and a verification section (see below). This is the same content structure defined in "Write the Challenge" — write it to disk instead of only showing it in chat when the user is meant to run and iterate on real files.
- `index.html`: standard Vite entry pointing at `/src/main.jsx`, with a `<title>` and a `<meta name="description">` naming the exercise.
- `src/main.jsx`: mounts `<App />` (or the scenario-named component) into `#root`, same shape as the root app's `main.jsx`.
- Provide any mock data (e.g. `src/ingredients.js`) as a plain module the component imports, framed as "a mock API response from the canonical backend datastore" — this lets the exercise test server-state-vs-local-state reasoning without a real network layer.

## Wire up npm scripts

Add two scripts to the practice repo's root `package.json`, keyed by the same slug used in the folder name:

```json
{
  "scripts": {
    "exercise:<slug>": "vite exercises/<slug>",
    "build:<slug>": "vite build exercises/<slug>"
  }
}
```

Reference these exact script names in the exercise's verification section so the commands are copy-pasteable:

```bash
npm run lint
npm run build:<slug>
npm run exercise:<slug>
```

Use whatever lint command the repo already defines (`npm run lint`) rather than assuming a specific linter — respect existing tooling instead of introducing a new one.

## Cleanup

Exercise folders accumulate across sessions; that's expected — they're the practice history, parallel to `gym-log/`. Don't delete completed exercises unless the user asks. When a workout is abandoned or replaced before the main set is scaffolded, skip this step entirely rather than leaving an empty folder and dangling npm scripts behind.
