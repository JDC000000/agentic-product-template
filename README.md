# agentic-product-template

Opinionated scaffold for a new agentic product, distilled from three sibling
repos that learned the same lessons three different ways:

- **stem-loops** — `JDC000000/stem-loops` (anchor exemplar: AGENTS.md shape,
  merge-safety rules M-1..M-7, the four-role Claude agent registry, the
  triple-gate auto-merge workflow, CODEOWNERS "narrow sensitive paths"
  pattern, secret convention).
- **the-wip** — `JDC000000/the-wip` <!-- TODO: review once the-wip and 24-hours-of-power are compared. -->
- **24-hours-of-power** — `JDC000000/24-hours-of-power` <!-- TODO: review once the-wip and 24-hours-of-power are compared. -->

## Source commit SHAs (Nov 2026)

What the current scaffold distils from, so future versions can diff against
the exact source trees:

- stem-loops `v3/main`: `ddbbae8869ad3d2e2ea159cf8b2c71eaa5802704`
  ("[v3-phase-0] ci: check TSD §N.M references in PR bodies resolve to headings (Fleet Move 4) (#65)")
- the-wip `main`: <!-- TODO: review once the-wip and 24-hours-of-power are compared. -->
- 24-hours-of-power `main`: <!-- TODO: review once the-wip and 24-hours-of-power are compared. -->

## How to use the template

```bash
gh repo create <new-product-name> \
  --template JDC000000/agentic-product-template \
  --public
```

After GitHub creates the new repo from this template, clone it locally and
walk `TEMPLATE-CHECKLIST.md` top-to-bottom — every item is something the
new product has to customize before it's a real product.

## What this template is

- `AGENTS.md` — canonical context file (shape; placeholders for product-specific content)
- `CLAUDE.md` — thin pointer to AGENTS.md
- `.github/CODEOWNERS` — narrow sensitive-paths pattern (no catch-all `*`)
- `.github/workflows/`
  - `auto-label-codeowners.yml` — label CODEOWNERS-touching PRs
  - `auto-merge.yml` — triple-gate auto-merge (requires `BOT_TOKEN` repo secret)
  - `ci.yml` — minimal placeholder (lint / test / TSD-refs)
  - `deploy.yml` — placeholder with Vercel + Fly.io examples commented out
- `.claude/agents/` — four behavioural role definitions (developer, code-reviewer, tester, security)
- `.claude/hooks.json` — minimal PostToolUse shape (Langfuse-ready placeholder)
- `docs/`
  - `agent-registry.md` — four-role table + handoff/boundary shape
  - `spikes/_template.md` — Gate 0 spike shape (six sections + decision)
  - `secret-convention.md` — server-side-only rules, generation, redaction
- `scripts/check_tsd_refs.py` — CI gate: PR body TSD §N.M refs resolve to headings
- `.env.example` — placeholder shape; never commit a real `.env`
- `TEMPLATE-CHECKLIST.md` — ten things a new product must customize

## What this template is NOT

- Not a working app. There is no `apps/`, no `packages/`, no `Makefile`,
  no stack choice. The template is scaffolding for the agent contract
  (AGENTS.md, CODEOWNERS, auto-merge, agent registry, spike template);
  the stack choice belongs to each product.
- Not opinionated about the deploy target. The `deploy.yml` placeholder
  shows Vercel + Fly.io as commented-out examples; swap them for whatever
  the product actually ships on.
- Not opinionated about the database. The `secret-convention.md` and
  CODEOWNERS rules use Postgres / Supabase shapes because that's the
  stem-loops baseline, but the shape generalizes.

## License

<!-- TODO: add a LICENSE file. -->
