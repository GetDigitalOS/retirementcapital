# Calculation Walkthrough

## Default scenario

**Inputs:** $50,000/yr income, 5% return, 3% inflation, 30 years

### How the capital figure is derived

The calculator discounts each future monthly withdrawal back to present value:

```
Month 1:  $4,166.67 / (1.004074)^0  = $4,166.67    (no discounting)
Month 2:  $4,166.67 / (1.004074)^1  = $4,149.76
Month 3:  $4,166.67 / (1.004074)^2  = $4,132.90
...
Month 13: $4,291.67 / (1.004074)^12 = $4,086.34    (year 2 income, inflation-adjusted)
```

Where `1.004074` is the monthly return rate derived from `(1.05)^(1/12)`.

The sum of all 360 present values = total capital needed.

### Balance simulation

Starting from the total capital:
1. Withdraw that month's income from the balance
2. Apply monthly return on the remaining balance
3. Repeat for 360 months
4. Balance reaches ~$0 at the end (by construction of the present-value sum)

### Key pattern: `Math.max(0, ...)` guard

The code floors balances and returns at zero to prevent negative values in edge cases (e.g., rounding or when actual returns fall short). See `script.js:65-66`.
