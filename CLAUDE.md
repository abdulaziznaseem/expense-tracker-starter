# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project context

Starter project for a Claude Code course: a React expense tracker that **intentionally** ships with a bug, poor UI, and messy code, to be fixed incrementally. Expect to refactor, not just patch.

## Commands

```bash
npm install        # required first; `vite` is not found without node_modules
npm run dev        # Vite dev server at http://localhost:5173
npm run build      # production build to dist/
npm run preview    # serve the built dist/
npm run lint       # ESLint (flat config in eslint.config.js)
```

There is no test runner configured yet.

## Architecture

- Vite 7 + React 19, plain JavaScript (`.jsx`), ES modules. No router, no state library, no backend, no persistence — data resets on reload.
- `src/main.jsx` mounts `<App />` in `StrictMode`.
- `src/App.jsx` is a thin container: it owns the `transactions` array (the only shared state, seeded with sample data) and a module-level `categories` constant, and composes three components:
  - `Summary.jsx` — props: `transactions`. Derives income / expenses / balance.
  - `TransactionForm.jsx` — props: `categories`, `onAdd`. Owns its input state; builds a complete transaction (id, parsed amount, today's date) and passes it up via `onAdd`.
  - `TransactionList.jsx` — props: `transactions`, `categories`. Owns type/category filter state and renders the filtered table.
- Data flows one way: state lives in `App`, children read it via props and report changes through callbacks. State used by only one component stays inside that component; lift it to `App` only when another component needs it.
- Components are flat files in `src/` (no subfolders). All styles are in `src/App.css`, keyed by class names the components use; global resets are in `src/index.css`.
- Transaction shape: `{ id, description, amount, type: "income" | "expense", category, date: "YYYY-MM-DD" }`. `amount` must be a **number** (form input is a string — convert with `parseFloat` before storing) or the `reduce` totals concatenate strings.

## Lint notes

`no-unused-vars` ignores names matching `^[A-Z_]`. `eslint-plugin-react-refresh` (Vite preset) warns when a file exports non-components alongside components.
