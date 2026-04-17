# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a React-based expense/finance tracker SPA, used as a starter project for the Claude Code course by Mosh Hamedani. It intentionally contains bugs, poor UI, and messy code that are meant to be fixed throughout the course.

## Commands

```bash
npm install       # Install dependencies
npm run dev       # Start dev server at http://localhost:5173
npm run build     # Production build
npm run lint      # Run ESLint
npm run preview   # Preview production build
```

## Architecture

- **Stack:** React 19 + Vite + ESLint (JavaScript, not TypeScript)
- **Entry:** `index.html` → `src/main.jsx` → `src/App.jsx`

There is no routing, no backend, and no persistence — state resets on page reload.

### Component Structure

- **`App.jsx`** — root component; owns the `transactions` array in state and passes it down
- **`Summary.jsx`** — receives `transactions`, computes `totalIncome`, `totalExpenses`, and `balance` internally
- **`TransactionForm.jsx`** — owns its own form state; calls `onAdd(transaction)` prop on submit
- **`TransactionList.jsx`** — receives `transactions`; owns filter state (`filterType`, `filterCategory`) internally

Transaction `amount` values are stored as numbers throughout (initial data and on add).

### Known Intentional Issues

- No delete transaction functionality despite `.delete-btn` CSS class existing in `App.css`
