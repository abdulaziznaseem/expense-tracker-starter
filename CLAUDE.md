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
- `src/App.jsx` is a single monolithic component holding everything: seed transaction data, form state, filter state, derived totals (income / expenses / balance), the add-transaction form, filters, and the transactions table. Styles live in `src/App.css` (global resets in `src/index.css`).
- Transaction shape: `{ id, description, amount, type: "income" | "expense", category, date: "YYYY-MM-DD" }`. Categories are a hard-coded array in `App.jsx`.

## Known issues in the starter code

- `amount` is stored as a **string** (both in seed data and from the form input), so the `reduce` totals concatenate strings instead of summing numbers.
- Seed entry "Freelance Work" has `type: "expense"` but `category: "salary"`.

## Lint notes

`no-unused-vars` ignores names matching `^[A-Z_]`. `eslint-plugin-react-refresh` (Vite preset) warns when a file exports non-components alongside components.
