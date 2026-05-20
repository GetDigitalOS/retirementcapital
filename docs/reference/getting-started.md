# Getting Started

## Prerequisites

- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server, build tool, or package manager required

## Running locally

```bash
# Option 1: open directly
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux

# Option 2: local dev server (if you have one)
npx serve .
# or
python -m http.server 8000
```

## Project structure

```
retirementcapital/
├── index.html       # Page markup and form
├── script.js        # All calculation and rendering logic
├── styles.css       # Styling
└── docs/            # Project documentation
```

## External dependencies

| Dependency | Version | Source | Purpose |
|------------|---------|--------|---------|
| Chart.js | latest (CDN) | `cdn.jsdelivr.net/npm/chart.js` | Line chart rendering |

No npm packages, no lock files. Chart.js is loaded from CDN at runtime.

## Key functions (script.js)

| Function | Purpose |
|----------|---------|
| `calculateCapital()` | Main entry point — reads inputs, runs calculation, renders all outputs |
| `formatCurrency(value)` | Formats number as USD string |
| `formatPercent(value)` | Formats decimal as percentage string |
| `toggleTable(view)` | Switches between annual and monthly table display |
