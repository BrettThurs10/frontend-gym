# Scaffold the Gym Log Viewer

Use this when `gym-log/` exists in the practice repository but nothing renders it yet — no root app, or an existing app that doesn't load `gym-log/*.md`.

Search first: look for an `import.meta.glob` call over `gym-log/*.md` anywhere in the repo. If one already exists, reuse and extend that app instead of scaffolding a new one. Do not overwrite an existing `src/App.jsx` that serves an unrelated purpose — ask the user before replacing it.

## Steps

1. If the repo has no `package.json`, scaffold a Vite + React app at the repo root:

   ```bash
   npm create vite@latest . -- --template react
   npm install
   ```

   If `package.json` already exists (for example, an exercise was created first), add only what's missing instead of re-scaffolding: `npm install react react-dom` and `npm install --save-dev vite @vitejs/plugin-react`, then add `vite.config.js`, `index.html`, and `src/main.jsx` if absent.

2. Add a `"dev": "vite"` script if one isn't already defined. Keep any existing `lint`, `build`, or `preview` scripts as-is.

3. Copy the mascot asset into the app's static folder: `cp assets/koala-sunglasses.png public/koala-sunglasses.png` (resolve `assets/` relative to this skill's own directory).

4. Create or replace `src/App.jsx` with a component along these lines — reproduce this shape rather than inventing a new one each time:

   ```jsx
   import { useMemo, useState } from 'react'
   import './App.css'

   const modules = import.meta.glob('../gym-log/*.md', {
     eager: true,
     query: '?raw',
     import: 'default',
   })

   const logs = Object.entries(modules)
     .map(([path, content]) => {
       const filename = path.split('/').at(-1)
       const isProfile = filename === 'PROFILE.md'
       const isActive = filename === 'ACTIVE_WORKOUT.md'
       const isBacklog = filename === 'BACKLOG.md'
       const isDatedLog = /^\d{4}-\d{2}-\d{2}\.md$/.test(filename)
       let label = filename.replace('.md', '')

       if (isProfile) label = 'Progress profile'
       else if (isActive) label = 'Active workout'
       else if (isBacklog) label = 'Training backlog'
       else if (isDatedLog) {
         label = new Intl.DateTimeFormat('en-US', {
           month: 'short',
           day: 'numeric',
           year: 'numeric',
         }).format(new Date(`${filename.replace('.md', '')}T12:00:00`))
       }

       return { id: filename, filename, content, isProfile, isActive, isBacklog, isDatedLog, label }
     })
     .sort((a, b) => {
       if (a.isActive) return -1
       if (b.isActive) return 1
       if (a.isProfile) return -1
       if (b.isProfile) return 1
       if (a.isBacklog) return -1
       if (b.isBacklog) return 1
       return b.filename.localeCompare(a.filename)
     })
   ```

   Do not add a Markdown dependency — render headings (`#`/`##`/`###`), `- ` lists, `|` tables, and inline `**bold**`/`` `code` ``/“smart quotes” with a small hand-rolled walker over the file's lines. This keeps the practice repo dependency-light, matching the rest of the skill's "don't introduce a library for a fundamentals exercise" stance.

5. In the rendered UI, include:
   - a labeled search input (`<label htmlFor="log-search">`) filtering entries by label and content;
   - a `<nav aria-label="Gym log entries">` of buttons, one per log, with `aria-current="page"` on the selected entry;
   - the koala mascot (`/koala-sunglasses.png`) with alt text `"Frontend Gym koala wearing sunglasses and a headband"`;
   - the selected log's content rendered in an `aria-live="polite"` region so screen readers announce switches.

6. Verify before telling the user it's ready: run `npm run build` and `npm run dev`, then confirm the profile, active workout, backlog, and at least one dated log all render and the search box filters correctly.
