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

All logic lives in a single monolithic `src/App.jsx` component: state management, filtering, financial calculations, and UI. There is no routing, no backend, and no persistence — state resets on page reload.

### Known Intentional Issues

- `totalIncome` and `totalExpenses` calculations use `reduce` on `t.amount` without numeric conversion (amounts are stored as strings)
- No delete transaction functionality despite `.delete-btn` CSS class existing in `App.css`
- All state and logic in one component with no modularization
