# Retirement Capital Calculator — Specification

## Purpose

Calculate the lump-sum capital required at retirement to fund a stream of inflation-adjusted monthly withdrawals over a specified number of years, assuming a constant after-tax annual return rate.

## Inputs

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| Initial Annual Income | number (USD) | $50,000 | First-year annual withdrawal amount |
| Annual After-Tax Return Rate | percent | 5% | Expected annual investment return after taxes |
| Annual Inflation Rate | percent | 3% | Expected annual inflation rate |
| Number of Retirement Years | integer | 30 | Duration of retirement in years |

## Calculation logic

1. **Monthly return rate** = `(1 + annualRate)^(1/12) - 1`
2. For each year `y` (1 to N):
   - Annual income = `initialIncome * (1 + inflationRate)^(y-1)`
   - Monthly income = annual income / 12
   - For each month `m` (1 to 12):
     - Months elapsed = `(y-1)*12 + m - 1`
     - Present value = `monthlyIncome / (1 + monthlyReturnRate)^monthsElapsed`
3. **Total capital needed** = sum of all present values
4. **Balance simulation**: starting from total capital, subtract monthly income then apply monthly return on remainder each month.

## Outputs

| Output | Description |
|--------|-------------|
| Total capital needed | Single currency figure displayed prominently |
| Line chart | Dual-axis: annual income (left) vs. account balance (right) over retirement years |
| Annual table | Year, total income withdrawn, total returns earned, ending balance |
| Monthly table | Year, month, monthly income, monthly return, ending balance |

## Behavior

- Calculation runs automatically on page load with default values.
- User can modify inputs and click "Calculate" to recompute.
- Annual table shown by default; monthly table available via toggle button.
- Account balance is floored at $0 (no negative balances).
