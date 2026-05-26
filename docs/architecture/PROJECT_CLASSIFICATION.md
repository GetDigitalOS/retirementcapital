# Project Classification — retirementcapital

| Field | Value |
|-------|-------|
| **Generated** | 2026-05-24 |
| **Method** | Auto-classified from project documentation |
| **Result** | **Tier 1 — Static/Marketing** (0/10 yes) |

## Assessment

| # | Question | Answer | Rationale |
|---|----------|--------|-----------|
| auth | User Authentication | No | No user accounts, login, or personalized data — purely a stateless client-side calculator. |
| data | Data Persistence | No | Explicitly no persistence; all state lives in the DOM with no backend or localStorage. |
| roles | Multi-Role Access | No | Single anonymous user interaction with no permission model of any kind. |
| integrations | Third-Party Integrations | No | Only Chart.js via CDN for rendering; no external APIs, payment processors, or services. |
| realtime | Real-Time Features | No | Calculations run on page load and button click — no websockets, live updates, or collaboration. |
| sensitive | Transaction Sensitivity | No | Inputs are hypothetical retirement figures computed locally; no PII, payments, or financial accounts are stored or transmitted. |
| scale | Scale Expectations | No | Static HTML/CSS/JS opened directly in a browser with no server load to scale. |
| team | Team Size | No | Tiny single-file project with no indication of multiple developers or team workflow. |
| longevity | Longevity | No | Simple calculator utility with no roadmap, phases, or maintenance plan suggesting multi-year evolution. |
| ai | AI/LLM Features | No | Pure deterministic financial math; no AI, LLM, or ML features present or planned. |

## Tier Determination

**Tier 1 — Static/Marketing**

- Yes count: 0/10
- Count-based tier: 1
- Final tier: 1 (max of count + escalation)
