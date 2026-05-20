# Roadmap

Potential improvements based on current codebase state. [VERIFY: confirm priorities with project owner]

## Near-term candidates

- **Input validation** — no validation on form inputs currently; negative or zero values can produce unexpected results
- **Responsive layout** — the flex layout uses inline styles and `max-width: 600px`; chart and tables are not mobile-friendly
- **Accessibility** — form lacks `<fieldset>`, ARIA attributes, and keyboard navigation support for the toggle buttons

## Medium-term candidates

- **Multiple scenarios** — allow comparing different return/inflation rate combinations side by side
- **Export** — download results as CSV or PDF
- **localStorage persistence** — save last-used inputs so they survive page refresh

## Long-term candidates

- **Tax-aware modeling** — different tax brackets for withdrawals, Roth vs. traditional accounts
- **Social Security / pension integration** — offset required capital by expected fixed income sources
- **Monte Carlo simulation** — model variable returns instead of a fixed rate
