# AGENTS.md — Canonical Context File

> This is the SINGLE CANONICAL context file for this repository.
> All agents (Claude Code, Cursor, Copilot, CI bots) must read this file first.
> CLAUDE.md is a thin pointer to this file only.

## Project Overview

<!-- TODO: fill in. One-paragraph description of what this product does, who it's for, and the single sentence you'd put on the landing page. -->

## External Documents

<!-- TODO: list every spec / PRD / TSD / RFC an agent must read before shipping work. -->

- **PRD**: `<path or link>`
  - Contains §… Handoff Rules for Claude Code
- **TSD**: `<path or link>`
  - Contains §… Data model
  - Contains §… Error codes
  - Contains §… State machine
  - Contains §… Anti-goals
  - Contains §… Build order

## FLAG Resolution (canonical context)

If any downstream spec or context file conflicts with AGENTS.md, **AGENTS.md wins**.
CLAUDE.md points here and contains nothing else.

## Architecture

<!-- TODO: fill in. Replace this placeholder tree with the real layout. -->

```
apps/<web>/        # web frontend
apps/<worker>/     # backend / worker
packages/types/    # shared types (codegen source of truth → generated consumers)
fixtures/          # CI fixtures — committed to git
evals/bugs.json    # regression tracking
```

## State Machine

<!-- TODO: pseudo-code of the primary state machine for this product. Example shape: -->

```
jobs.status:  queued → running → done
                     → failed
```

Active stages emit events (`started`, `completed`, `failed`) to an append-only event table.

## Error Codes

<!-- TODO: fill in the closed set of error codes this product emits. One row per code. -->

| Code | When |
|------|------|
| `EXAMPLE_ERROR` | placeholder |

## Build Order

<!-- TODO: numbered list — Foundation → core behaviour → polish → production. -->

1. **Foundation** — schema, migrations, health, types codegen, CI green
2. **Core behaviour** — the one thing this product does
3. **UX Polish** — frontend / status / download
4. **Production/Ship** — rate limits, logging, deploy pipeline

## Anti-Goals (universal — do NOT repeat these mistakes)

These are universal across every product built from this template. Product-specific
anti-goals are additive — append them below, never remove these.

- Do NOT expose secrets to the browser. All API tokens are server-side only.
- Do NOT commit a secret (API key, token, webhook signing secret, DB password) to git.
  Only `.env.example` is committed.
- Do NOT prefix a real secret with `NEXT_PUBLIC_` (or any framework's client-exposed
  prefix). That prefix is reserved for non-secret public config only.
- Do NOT call paid third-party APIs from the browser or from an untrusted edge. All
  paid-API calls originate server-side, from an authenticated code path.
- Do NOT hard-code URLs, bucket names, or project IDs. Always read from env vars.
- Do NOT skip the migration runner. Always run `make migrate` (or equivalent) before
  `make dev` — the local DB is expected to be up to date.
- Do NOT block the event loop in the server/worker. Long-running work goes to a
  thread pool or a queue.
- Do NOT hand-edit generated code (types, schemas, API clients). Re-run the codegen
  and commit both the source change and the regenerated artefact.
- Do NOT silently disable a test to make CI green. A `skip` without a bug reference
  is a regression hiding in plain sight.
- Do NOT weaken CI, branch protection, or CODEOWNERS to make your own PR merge.
  Self-approval deadlocks have a documented resolution (service-account path — see
  `TEMPLATE-CHECKLIST.md`).

## Secret Convention

**All secrets are server-side only. Never in the browser. Never requested from users.**

<!-- TODO: fill in the product's secret table. Keep the shape below. -->

| Variable | Where used | How to get |
|----------|-----------|------------|
| `<SECRET_NAME>` | `<worker only | web (server)>` | `<provider dashboard URL>` |

See `.env.example` for the full list with documentation.
See `docs/secret-convention.md` for the generation + runtime rules.

## Make Targets

<!-- TODO: fill in. Keep the shape below; strip rows that don't apply. -->

| Target | What it does |
|--------|-------------|
| `make dev` | Spin up the stack locally |
| `make types` | Run codegen AND check for drift |
| `make types-generate` | Run codegen only (no drift check) |
| `make test` | Run the full test suite |
| `make lint` | Run all linters |
| `make deploy-web` | Deploy the web app |
| `make deploy-worker` | Deploy the worker |
| `make migrate` | Run database migrations (idempotent) |
| `make install` | Install all deps |

## Type Source of Truth

<!-- TODO: name the one file that is the source of truth for types, and the codegen path. -->

`<path/to/source-of-truth>` — is the SINGLE SOURCE OF TRUTH for all types.

- Any schema change → edit source → run `make types-generate` → commit both
- CI (`types-drift` job) fails if generated types are out of sync
- Consumers import from `<generated package name>`

## Handoff Rules

<!-- TODO: product-specific handoff rules go here. The ten below are universal starters. -->

1. Always read AGENTS.md first. It supersedes all other context.
2. Run `make install` before starting any work session.
3. Run `make dev` to verify the full stack is working before making changes.
4. Run `make lint` and `make test` before committing.
5. Never edit generated code by hand — re-run codegen.
6. Follow the build order — do not skip ahead to UX before the core behaviour works.
7. All new secrets go in `.env.example` with a comment explaining how to obtain them.
8. Regression bugs go in `evals/bugs.json`.
9. The state machine is fixed — do not add or rename statuses without a spec change.
10. Every PR that touches a CODEOWNERS path gets reviewed by a human.

## Merge Safety Rules

<!-- TODO: review once the-wip and 24-hours-of-power are compared — the rule set below is distilled from stem-loops's §11 M-1..M-7 incident log. Keep the rules that apply; prune the ones that don't. -->

**M-1 (hard): `--admin` must not bypass a FAILING status check.** Admin bypass is
for the CODEOWNERS / required-review layer only. If any check-run is red OR pending
OR absent, do not merge — fix CI first.

**M-2 (hard): reviewers must verify CI is green** before approving. A reviewer who
cannot run CI states that explicitly so the author re-checks before promoting.

**M-3 (medium): migrations with managed-object references need stubs.** When a
migration references anything your managed Postgres provider owns (`auth.*`,
`storage.*`, `realtime.*`, helper fns), include a stub so local bare-Postgres also
runs the migration.

**M-4 (medium): no cross-PR TYPE ↔ CODE drift.** When one PR adds a type that maps
to a column value, the same PR includes (or references by SHA) the migration that
sets those values.

**M-5 (process): hotfix PRs land before any dependent work merges.** If a merged
PR breaks CI, all other work blocks until the hotfix is on main.

**M-6 (process): the author of PR #N is a self-approval deadlock on #N.** For a
solo-dev repo, every CODEOWNERS-protected PR authored by the owner needs an
`--admin` bypass combined with a green-CI check.

**M-7 (process): service-account path for agent merges.** The clean structural fix
for the M-6 deadlock is a dedicated bot GitHub account with push + review
permissions. Track the one-time setup in `TEMPLATE-CHECKLIST.md`.
