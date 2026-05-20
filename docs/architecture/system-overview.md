# System Overview

## What is this?

A client-side **Retirement Capital Calculator** — a single-page web application that computes how much capital a person needs at retirement to fund inflation-adjusted annual withdrawals over a given number of years.

## Tech stack

| Layer | Technology |
|-------|-----------|
| Markup | Plain HTML5 (`index.html`) |
| Styling | Plain CSS (`styles.css`) |
| Logic | Vanilla JavaScript (`script.js`) |
| Charting | Chart.js (CDN) |

No build step, no bundler, no server-side code. Open `index.html` in a browser and it works.

## Architecture

```
Browser
 └─ index.html
      ├─ styles.css      (layout + form styling)
      ├─ Chart.js (CDN)  (line chart rendering)
      └─ script.js       (all calculation + DOM logic)
```

### Data flow

1. User enters 4 inputs: initial annual income, return rate, inflation rate, retirement years.
2. `calculateCapital()` runs on page load (with defaults) and on button click.
3. Calculation loop:
   - Converts annual return rate → monthly rate.
   - For each month across all retirement years, computes the inflation-adjusted monthly income and its present value.
   - Sums present values → **total capital needed**.
4. A second pass simulates the account balance over time (monthly withdrawals + monthly returns).
5. Results rendered as:
   - A summary line ("Total Retirement Capital Needed: $X").
   - A Chart.js dual-axis line chart (income vs. account balance by year).
   - Toggle-able annual and monthly data tables.

### Key design decisions

- **Beginning-of-period withdrawal model**: income is withdrawn at the start of each month, then returns accrue on the remaining balance.
- **Annual inflation step**: income increases at the start of each year (not monthly compounding of inflation).
- **No persistence**: all state lives in the DOM; nothing is saved to localStorage or a backend.
