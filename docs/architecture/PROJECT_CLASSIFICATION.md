# Project Classification — retirementcapital

| Field | Value |
|-------|-------|
| **Generated** | 2026-03-22 |
| **Method** | Auto-classified from project documentation |
| **Result** | **Tier 1 — Static/Marketing** (0/10 yes) |

## Assessment

| # | Question | Answer | Rationale |
|---|----------|--------|-----------|
| auth | User Authentication | No | No user accounts or login — it's a purely client-side calculator with no personalization. |
| data | Data Persistence | No | No database or persistence; all state lives in the DOM and nothing is saved. |
| roles | Multi-Role Access | No | No user roles or permissions — everyone sees the same single-page calculator. |
| integrations | Third-Party Integrations | No | Only external dependency is Chart.js via CDN; no APIs, payment processors, or services. |
| realtime | Real-Time Features | No | No live updates, websockets, or collaborative features — calculations run on button click. |
| sensitive | Transaction Sensitivity | No | No data is transmitted or stored; financial inputs exist only in the browser session. |
| scale | Scale Expectations | No | Static client-side HTML/CSS/JS with no server — scale is irrelevant. |
| team | Team Size | No | Simple three-file project with no build system, suitable for a single developer. |
| longevity | Longevity | No | A self-contained calculator with no dependencies to maintain and no evolving feature set. |
| ai | AI/LLM Features | No | Pure mathematical calculation with no AI or LLM features. |

## Tier Determination

**Tier 1 — Static/Marketing**

- Yes count: 0/10
- Count-based tier: 1
- Final tier: 1 (max of count + escalation)
