# Tech Stack

| Layer | Choice | Version | Notes |
|-------|--------|---------|-------|
| Language | TypeScript | latest stable | `strict` mode on |
| UI library | React | latest stable | Function components and hooks only |
| Build tool | Vite | latest stable | Dev server via `npm run dev` |
| Routing | React Router | latest stable | Two routes: `/` and `/pantry` |
| Styling | Plain CSS | | One file, `src/styles.css`. No CSS framework, no Tailwind |
| Data model | TypeScript type `Prompt` | | Fields: `id`, `title`, `text`, `tag`, `createdAt` |
| Storage | React state in `App` | | In memory only. No backend, no localStorage |
| Testing | Vitest + React Testing Library | latest stable | jsdom environment, `@testing-library/jest-dom` matchers |
| Package manager | npm | | Node.js current LTS |

Versions are pinned by the lockfile when the project is set up.

Run tests with `npm test`, which must run `vitest run` (one pass, no watch mode).
