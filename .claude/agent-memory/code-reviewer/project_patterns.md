---
name: Project Patterns
description: Recurring code issues, conventions, and architectural observations across all source files (App, Summary, TransactionForm, TransactionList, SpendingChart)
type: project
---

## Stack
React 19 + Vite, JavaScript (no TypeScript), ESLint with react-hooks plugin. No routing, no backend, state resets on reload.

## Conventions observed
- Functional components only, named exports at bottom of file
- `categories` array is duplicated in both TransactionForm and TransactionList — should be extracted to a shared constants file
- Event handlers named with `handle` prefix (handleSubmit, handleAdd, handleDelete) — consistent
- Transaction amounts stored as numbers throughout

## Recurring issues found (first full review, 2026-04-17)

### Bugs / Correctness
- **Summary.jsx line 16/20/24**: Dollar amounts rendered with template literals (`${totalIncome}`) — for non-integer amounts (e.g. 1570.5) this will show raw floats like `$1570.5` instead of `$1,570.50`. Need `toLocaleString('en-US', { style: 'currency', currency: 'USD' })` or `toFixed(2)`.
- **Summary.jsx**: No guard against empty transactions array (works but produces $0, which is fine — not a bug per se).
- **TransactionForm.jsx line 13**: Validation only checks truthiness of `amount`, not that it is a positive number. A user can submit amount=0 or a negative number.
- **TransactionForm.jsx line 18**: `Number(amount)` can produce NaN if the input is somehow non-numeric despite `type="number"` — no NaN guard.
- **TransactionList.jsx line 54**: Inline `window.confirm` inside JSX onClick — couples UI logic to the browser confirm dialog, makes testing hard, and is a UX anti-pattern.
- **App.jsx line 21**: `handleAdd` uses stale closure over `transactions` (`[...transactions, transaction]`). Should use functional updater form: `setTransactions(prev => [...prev, transaction])`.
- **App.jsx line 25**: `handleDelete` same stale closure issue — should use `setTransactions(prev => prev.filter(...))`.
- **SpendingChart.jsx line 42**: Uses array index as `key` for `<Cell>` elements. Since data order can change as transactions are added/deleted, this can cause rendering glitches. Should use `entry.name` as the key.

### Readability
- **TransactionList.jsx line 54**: 115-character inline onClick handler — should be extracted to a named `handleDelete` function inside the component.
- **TransactionList.jsx line 50-51**: Inline ternary for class and prefix is readable but could be a helper for consistency.
- **Summary.jsx**: No currency formatting — raw numbers shown.
- **TransactionForm.jsx**: Category `<option>` labels are lowercase raw strings (e.g. "food") — should be capitalized for display.

### Maintainability
- `categories` array defined in both `TransactionForm.jsx` (line 3) and `TransactionList.jsx` (line 3) — exact duplication. Should live in `src/constants.js` or `src/categories.js`.
- No empty-state UI in TransactionList when filteredTransactions is empty — the table just renders with no rows.
- SpendingChart already handles its own empty state (returns null) — good pattern to follow in TransactionList.

### Performance
- `handleAdd` and `handleDelete` in App.jsx are recreated every render. For this project scale that is fine, but wrapping in `useCallback` would be the correct pattern to demonstrate.
- Summary computations (totalIncome, totalExpenses, balance) run on every render. `useMemo` would be the correct teaching moment here.

## What is done well
- SpendingChart empty-state guard (`if (data.length === 0) return null`) is clean.
- TransactionList filter logic is clear and simple.
- TransactionForm resets all fields after submit — correct behavior.
- `key` props are present and correct on category option lists.
- Component responsibilities are well separated for a project this size.
