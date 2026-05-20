# Docs Maintenance Rules (for AI + Humans)

These rules are binding for every Claude Code session and every code change in this project.

## Always
- Read the entire /docs folder before starting any task.
- Update docs only when the change matters for a future developer or team handoff.
- Prefer editing existing files over creating new ones.
- Keep additions concise — prefer <10 lines when possible.
- Use tables, lists, and diagrams only when they reduce total words.
- Flag documentation drift explicitly: "Documentation update required: [file] — [what changed]"

## Never
- Add tutorial-style fluff, repetition, or overview sections that already exist elsewhere.
- Create a new subfolder unless a genuinely new category appears (e.g., new `security/` or `deployment/` concern with 3+ related files).
- Let docs get out of sync with code — if you changed it, update the doc in the same session.
- Delete history — use `[DEPRECATED]` markers or move to `docs/archive/`.

## Approved top-level structure (do not add folders without explicit approval)
- `architecture/`   — system design, ADRs, data models, diagrams
- `examples/`       — working code samples, patterns, usage demos
- `planning/`       — roadmaps, milestones, feature specs in progress
- `reference/`      — API docs, config schemas, environment variables, quick-ref tables
- `specifications/` — finalized feature specs, acceptance criteria
- `archive/`        — deprecated content (never delete, just move here)
- `docs-maintenance.md` — this file

## File naming
- Lowercase, hyphen-separated: `auth-flow.md`, not `AuthFlow.md`
- ADRs: `adr-001-framework-choice.md` (zero-padded number + slug)

## When to create a new file vs. edit an existing one
- Edit existing: adding a section, updating a pattern, correcting outdated info
- New file: entirely new topic with no existing home, OR existing file exceeds ~200 lines and has a clean split point
- When in doubt: edit existing
