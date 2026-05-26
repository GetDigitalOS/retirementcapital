# Compliance Report — Retirement Capital Calculator

**Date:** 2026-05-23
**Tier:** 1 — Static/Marketing (per `PROJECT_CLASSIFICATION.md`)
**Framework Version:** Universal Web Development Principles v4.03.01 (`canonical/references/Universal_Web_Development_Principles.md`)

## Summary

| Status | Count |
|--------|-------|
| ✅ Met | 9 |
| ⚠️ Partial | 11 |
| ❌ Not Met | 14 |
| ⬜ Not Applicable | 5 |

**Overall Compliance:** 59% of applicable principles met or partially met (20/34). Down from 71% at the 2026-03-22 audit because the v4 framework adds SEO Operations and Observability bands to Tier 1 (GSC, Bing/IndexNow, Plausible, MXToolbox gate) that this project does not meet.

## Critical Gaps (Fix Before Launch)

1. **🔴 Project Hub tier mismatch** — Hub registry has `"tier": "unclassified"` while `PROJECT_CLASSIFICATION.md` says Tier 1. `existing_state.claude_md` and `existing_state.settings_json` are both `false` in the registry but `CLAUDE.md` is present and `.claude/` is scaffolded (template only — no real `settings.json`). Run `/scaffold` or hub re-sync to reconcile.
2. **🔴 Broken Cloudflare Pages deploy workflow** — `.github/workflows/deploy.yml` calls the shared `deploy-cloudflare-pages.yml` with `project-name: ""` (empty), `build-command: "npm run build"`, and `output-directory: "dist"`. There is no `package.json`, no build script, and no `dist/` folder. The first push to `main` or `dev` will fail. Either fix the workflow to deploy static files directly (no build, output `.`) or add a real `package.json` + build step.
3. **🔴 No server-side input validation pathway** — All inputs go through `parseFloat`/`parseInt` with no bounds. Negative income, zero years, NaN, and >100% rates produce garbage outputs. HTML inputs have no `min`/`max`. (`script.js:20-23`, `index.html:17-26`)
4. **🔴 No semantic landmarks** — Page is `<body> > <h1> > <div>`. No `<main>`, `<header>`, `<footer>`, `<section>`, or `<h2>`s. Screen-reader users cannot navigate by region. (`index.html`)
5. **🔴 No focus indicators** — CSS has no `:focus-visible` rules. Keyboard users cannot see where they are. Toggle buttons are injected via `innerHTML` with no `aria-pressed` state. (`styles.css`, `script.js:172-177`)
6. **🔴 Missing SEO meta + Open Graph** — No `<meta name="description">`, no `og:*`, no `twitter:card`, no canonical URL. (`index.html:4-10`)
7. **🔴 Not responsive** — `body { max-width: 600px }` plus a hardcoded two-column `display: flex` clips the chart and tables on mobile. No `@media` queries. The 400×400 canvas overflows narrow viewports.
8. **🔴 No analytics / no pre-launch DNS gate** — Tier 1 v4 requires Plausible (`NEXT_PUBLIC_PLAUSIBLE_DOMAIN`) and an MXToolbox/Google Workspace Toolbox pre-launch check for any custom domain. Neither exists.

## Detailed Results

### Tier 1: Security Fundamentals

- ⬜ **HTTPS Everywhere** — N/A locally; Cloudflare Pages enforces HTTPS automatically when the deploy succeeds. Workflow currently broken (see Critical #2).
- ⬜ **Security Headers** — N/A in repo; would be set via `_headers` on Cloudflare Pages. No `_headers` file exists. CSP/HSTS/X-Frame-Options not configured.
- ❌ **Input Validation** — `parseFloat`/`parseInt` only, no guard against negatives, zero years, NaN, or absurd rates. No `min`/`max` HTML attributes. (`script.js:20-23`)
- ✅ **Dependency Hygiene** — Single CDN dependency (Chart.js via jsdelivr). No npm, so no `npm audit` exposure, but also no version pin (loads `chart.js` latest — silent breakage risk).

### Tier 1: Design System Basics

- ❌ **Semantic Tokens** — None. Raw hex values (`#4CAF50`, `#45a049`, `#333`, `#ccc`) used directly. No CSS custom properties.
- ❌ **Token Hierarchy** — No primitives/semantic/component split.
- ⚠️ **Consistent Spacing Scale** — Ad-hoc `20px`/`10px`/`15px`/`8px`. Not a deliberate scale.
- ⚠️ **Typography Scale** — One font (Arial fallback), no defined size scale beyond `1.2em` on the result line.
- ⬜ **Icon System** — N/A; no icons in use.
- ❌ **Responsive Breakpoint Strategy** — No breakpoints, no `clamp()`, no container queries. Mobile is broken.

### Tier 1: Performance

- ✅ **Minimal Payload** — HTML ~1.4KB, CSS ~0.5KB, JS ~6.8KB, plus Chart.js CDN (~200KB).
- ⚠️ **Render-Blocking Resources** — Chart.js loaded in `<head>` with no `defer`/`async` (`index.html:9`). Blocks first paint.
- ⬜ **Asset Optimization** — N/A; no images.
- ⬜ **Lazy Loading** — N/A; no offscreen images or below-fold heavy resources.
- ✅ **CDN Delivery** — jsdelivr for Chart.js; Cloudflare Pages will edge-serve the static files.
- ⚠️ **Core Web Vitals Targets** — Not measured. No Lighthouse CI configured. Render-blocking Chart.js and unpinned CDN risk LCP regressions.

### Tier 1: Accessibility (WCAG 2.1 AA)

- ✅ **Language Attribute** — `<html lang="en">` (`index.html:3`).
- ✅ **Form Labels** — All 4 inputs have associated `<label for="...">` (`index.html:16-26`).
- ❌ **Semantic HTML / Landmarks** — No `<main>`, `<header>`, `<footer>`. Flat structure.
- ❌ **Keyboard Navigation & Focus Indicators** — No `:focus-visible` styles in `styles.css`. Toggle buttons created via `innerHTML` (`script.js:172-177`) have no `aria-pressed`, no role hint.
- ⚠️ **Heading Hierarchy** — Single `<h1>`. Results, chart, and tables have no `<h2>` to delineate sections for screen-reader navigation.
- ❌ **Color Contrast** — Button text white on `#4CAF50` has ratio ≈3.2:1 — fails AA for normal text (needs 4.5:1). Primary `#333` body on white is fine (~12.6:1).
- ⬜ **Alt Text** — N/A; no `<img>` elements.
- ❌ **Reduced Motion** — Chart.js animations run unconditionally. No `prefers-reduced-motion` handling. Initialize Chart.js with `animation: false` when the media query matches.

### Tier 1: SEO Fundamentals

- ❌ **Semantic HTML for Search** — Title is present and descriptive. Meta description is missing. Heading hierarchy lacks `<h2>` structure.
- ❌ **Open Graph & Social Tags** — None.
- ⬜ **Structured Data** — Optional for a calculator; reasonable to omit, but a `WebApplication` JSON-LD block would be appropriate.
- ❌ **Technical SEO** — No `robots.txt`, no `sitemap.xml`, no canonical link tag.
- ⚠️ **Performance as SEO** — Render-blocking Chart.js will hurt LCP. Untested.

### Tier 1: SEO Operations (v4)

The hub `stack.seo` field for this project is `"required"`, so these apply:

- ❌ **Google Search Console** — Not configured (no verification meta, no DNS record evidence).
- ❌ **Bing Webmaster Tools + IndexNow** — No IndexNow key file at root.
- ❌ **Programmatic IndexNow Pings** — N/A without an SEO package; this is a hand-rolled static project.
- ❌ **Pre-Launch Validation Gate** — No evidence of Rich Results Test run.
- ❌ **Canonical SEO Implementation** — `@getdigitalos/seo` not used; no ADR documenting the deviation.

### Tier 1: Observability & Diagnostics (v4.02)

- ❌ **Privacy Analytics (Plausible)** — Not installed. `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` not set.
- ❌ **Pre-Launch DNS + Email Diagnostic Gate** — No `launch-checklist.md` and no record of MXToolbox/Google Workspace Toolbox checks. Required for any custom domain at launch.

### Tier 1: Code Quality

- ⚠️ **Separation of Concerns** — CSS file is separate from JS, but `index.html` has 4 inline `style=""` attributes (lines 13, 14, 33, 38) and 3 inline `onclick="..."` handlers (lines 28, 173, 174). Mixed concerns.
- ⚠️ **DRY** — Annual and monthly table-building blocks (`script.js:159-169`) are near-duplicates differing only in columns. Could be one helper.
- ✅ **Semantic Naming** — `calculateCapital`, `formatCurrency`, `monthlyReturnRate`, etc., describe purpose.
- ❌ **Mobile-First CSS** — No mobile base; desktop-only layout with `max-width: 600px` clipping the two-column flex.

### Tier 1: DevOps Basics

- ❌ **Automated Deployment** — `.github/workflows/deploy.yml` exists but is **broken**: empty `project-name`, references `npm run build` and `output-directory: dist` with no `package.json`/no build/no `dist/`. First push will fail.
- ❌ **Preview Deployments** — Workflow does target `dev` branch but is non-functional for the reason above.
- ⚠️ **Environment Separation** — Branch model is there (`main`/`dev`); deploy not actually working.
- ❌ **Domain & DNS Management** — No custom domain documented. Hub registry has no `live_url`/`preview_url` for this project (the default `<project>.pages.dev` is implied but unconfirmed).
- ✅ **Version Control** — Git with meaningful commit history on `main` (5+ commits).

### Tier 1: UX Fundamentals

- ❌ **Responsive Design** — See Critical #7.
- ❌ **Error Prevention** — `required` only; no validation messages, no `min`/`max`, no inline feedback.
- ⚠️ **Loading States** — Calculation is synchronous and fast, so loading UX is arguably moot — but there is no visual confirmation that "Calculate" succeeded other than the result text changing.
- ✅ **Consistent Patterns** — The toggle buttons and form controls behave consistently within the page.

### Cross-Cutting: Privacy & Data Protection

- ✅ **No Data Collection** — Pure client-side; no analytics, cookies, or backend transmission today.
- ⚠️ **CDN Privacy** — Chart.js from jsdelivr means user IPs go to a third party. Acceptable for a non-EU audience but should be acknowledged for GDPR. Switching to a pinned, self-hosted Chart.js bundle eliminates this and the version-drift risk.

### Cross-Cutting: AI/LLM Integration

- ⬜ **Not Applicable** — No AI features.

### Cross-Cutting: Dependency Management

- ❌ **No Lock File / No Version Pin** — Chart.js URL is `chart.js` (latest). Any breaking major release silently lands in production. Pin to `chart.js@4.4.7` (or current stable) and add SRI hash, or vendor the file.
- ✅ **Minimal Dependencies** — One external runtime dep.

### Cross-Cutting: Structural Integrity (v2.02+)

- ⚠️ **Root-cause posture** — `Math.max(0, ...)` is used in several places (`script.js:64-66, 91`) to clamp negative balances. This masks the underlying issue that the simulation will go negative when capital is insufficient. The clamp should be paired with a "ran out of money in year N" surfaced to the user, not silently zeroed.
- ⚠️ **Deploy workflow** — Broken from the day it was added (see Critical #2). Classic infra drift — the workflow was scaffolded against a Node-build assumption that doesn't fit this project. Either repair or remove.

### Cross-Cutting: Project Hub Registration

- ✅ **Registered in Project Hub** — Entry exists at `projects.json` for `C:/dev/business/financial-tools/retirementcapital`.
- ❌ **Tier matches classification** — Hub `"tier": "unclassified"`; `PROJECT_CLASSIFICATION.md` says **Tier 1**. Re-classify and update the registry.
- ✅ **Git remote configured** — Implied from `git remote -v` (`https://github.com/GetDigitalOS/retirementcapital.git`), though the hub `git_remote` field for this entry is **missing**. Add it.
- ✅ **Dev port assigned** — `dev_port: 3207` (aligned-wealth range).
- ⚠️ **Status is current** — `status: "development"` is plausible but the deploy workflow is broken, so "development" is the right state. Existing-state flags are stale (`claude_md: false` / `settings_json: false` but CLAUDE.md is present; `.claude/` is scaffolded with `settings.local.json.template` only — no actual `settings.json`).

## Recommendations (Prioritized)

### 🔴 Critical (Security / Compliance / Will-not-deploy)

1. **Repair or replace the deploy workflow.** `.github/workflows/deploy.yml:11-14` references a Node build that does not exist. Either (a) switch to a static-files deploy (set `build-command: ""`, `output-directory: "."`, fill `project-name: "retirementcapital"`), or (b) add a real `package.json` with a `build` step and `dist/` output.
2. **Reconcile Project Hub registration.** Update `registry/projects.json` entry: set `"tier": 1`, refresh `existing_state` (`claude_md: true`, `settings_json: false` until a real one is added), add `"git_remote": "https://github.com/GetDigitalOS/retirementcapital.git"`. Run `/scaffold` if the canonical flow is preferred.
3. **Add input validation.** Reject `≤0` income, `≤0` years, rates outside a sane range (e.g., 0%-50%), and NaN. Surface a clear error message; do not call Chart.js with bad data. (`script.js:18-23`, `index.html:17-26`)
4. **Wrap content in semantic landmarks and add section headings.** `<header><h1></header><main>...</main>` with `<h2>` for Results, Chart, and Tables. (`index.html`)
5. **Add focus styles and ARIA state for toggle buttons.** `:focus-visible { outline: 2px solid currentColor; outline-offset: 2px }` in CSS; create the toggle buttons with `aria-pressed="true|false"` instead of injecting via `innerHTML`. (`styles.css`, `script.js:172-177`)
6. **Add meta description, Open Graph, and canonical.** Description + `og:title`/`og:description`/`og:image` + `<link rel="canonical">`. (`index.html:4-10`)
7. **Make it responsive.** Replace the `display: flex` outer container with a CSS grid that collapses to one column under 768px; remove `body { max-width: 600px }` (it clips the chart). Mobile-first base styles, then `@media (min-width: 768px)` for the side-by-side layout.

### 🟡 Important (Quality/Reliability)

8. **Pin Chart.js + add SRI.** Change `cdn.jsdelivr.net/npm/chart.js` to `cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.js` with `integrity` + `crossorigin="anonymous"`, and add `defer` to unblock rendering. (`index.html:9`)
8. **Remove inline styles and inline event handlers.** Move the 4 inline `style=""` to CSS classes; replace `onclick=""` with `addEventListener` in `script.js`. (`index.html:13, 14, 28, 33, 38`; `script.js:173-174`)
9. **Stop generating buttons via `innerHTML`.** Use `document.createElement`/`textContent` for the toggle buttons and table cells (`script.js:160, 168, 172`). Today's data is internal, but the pattern is a foot-gun the moment external data enters.
10. **Surface "ran out of money" instead of clamping.** If `currentBalance` would go negative, display the year/month at which the simulation depletes rather than zeroing silently. (`script.js:64-66`)
11. **Add a launch checklist.** `docs/planning/launch-checklist.md` covering Lighthouse target, Rich Results Test, MXToolbox check (if a custom domain is used), and a manual screen-reader pass.
12. **Add design tokens.** Even at Tier 1, a few CSS custom properties (`--color-action`, `--color-text`, `--space-1`-`-4`) replace the magic hex values and pave the way for Tier 2 work.

### 🟢 Recommended (Best Practice)

13. **Add Plausible.** `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` is meaningless without a framework, but a one-line `<script defer data-domain="..." src="https://plausible.io/js/script.js">` satisfies the Tier 1 v4 observability band and adds no cookies.
14. **Add unit tests for the math.** Extract `calculateCapital` into a pure function (`income, returnRate, inflationRate, years → { totalCapital, monthlyTable, annualTable }`) and add a tiny test file. A `package.json` + Vitest is overkill; even a `tests.html` runner is enough at Tier 1.
15. **Add a `_headers` file for Cloudflare Pages.** CSP (locked to `'self'` + jsdelivr), `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` minimal.
16. **Add `<canvas>` text alternative.** `aria-label="Line chart: annual income and account balance across retirement years"` on `<canvas id="chart">` and a text summary nearby for screen-reader users.
17. **Reduced-motion support.** Initialize Chart.js with `animation: window.matchMedia('(prefers-reduced-motion: reduce)').matches ? false : undefined`.
18. **Pre-pin GSC + Bing verification meta tags** if a custom domain is on the roadmap.

## Next Review

Recommended next compliance check: **2026-08-23** (quarterly), or immediately before the first public deploy — whichever comes first.
