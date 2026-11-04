# TEMPLATE-CHECKLIST.md

Walk this list top-to-bottom the first time you create a new product from
`JDC000000/agentic-product-template`. Every item is scaffolding the template
left generic that the product has to make specific. Check items off in the
new product's own copy of this file as you go, then delete the file once
every item is green.

> TODO: review once the-wip and 24-hours-of-power are compared.

## 1. Repo name and identity

- [ ] Replace `<product>` placeholders in `CLAUDE.md`, `AGENTS.md`, and
      `docs/agent-registry.md` with the real product name.
- [ ] Set `description` on the GitHub repo (`gh repo edit --description ...`).
- [ ] Decide whether the repo is public or private; adjust visibility if the
      `--template` default was wrong.

## 2. Vercel project id

- [ ] Create the Vercel project (`vercel link` inside `apps/web/`, once that
      app exists) or import via the Vercel dashboard.
- [ ] Add `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID` to the repo
      secrets (`gh secret set VERCEL_TOKEN ...`).
- [ ] Un-comment the `deploy-web` job in `.github/workflows/deploy.yml`.

## 3. Fly app name

- [ ] Create the Fly app (`fly apps create <name>`) or pick a different
      worker target (Render, Railway, Cloud Run, etc.).
- [ ] Add `FLY_API_TOKEN` to the repo secrets.
- [ ] Un-comment the `deploy-worker` job in `.github/workflows/deploy.yml`
      (or swap it for the chosen worker platform's action).

## 4. Supabase / database project ref

- [ ] Create the database project (Supabase, Neon, Fly Postgres, etc.).
- [ ] Add `DATABASE_URL` and `DATABASE_POOL_URL` to the repo secrets and to
      the deploy-target environment.
- [ ] If using Supabase, also wire the stub pattern (M-3 in AGENTS.md) so
      bare-Postgres CI can run managed-object migrations.

## 5. CODEOWNERS paths

- [ ] Open `.github/CODEOWNERS` and replace every `@YOUR_GITHUB_HANDLE` with
      the actual owner handle (individual or team).
- [ ] Prune paths that don't exist in this product (e.g. no `apps/worker/`
      → remove the `apps/worker/*` entries).
- [ ] Update the alternation regex in `.github/workflows/auto-label-codeowners.yml`
      to match the current CODEOWNERS paths exactly (the file's header
      explains the invariant: regex == CODEOWNERS).

## 6. Branch protection rules

- [ ] On `main`, enable:
  - Require a pull request before merging (1 review)
  - Require review from Code Owners
  - Require status checks to pass (pick the actual CI job names once CI
    runs once; GitHub only lists check names it has seen)
  - Require branches to be up to date before merging
  - Do not allow bypassing the above settings (unless you want `--admin`
    to be the normal path — see AGENTS.md §M-1).

## 7. GitHub secrets

- [ ] `BOT_TOKEN` — fine-grained PAT for the bot service account (needed
      by `.github/workflows/auto-merge.yml`). See AGENTS.md §M-7 for the
      service-account rationale.
- [ ] `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, and any
      `STRIPE_PRICE_*` ids — if the product bills.
- [ ] `LANGFUSE_PUBLIC_KEY`, `LANGFUSE_SECRET_KEY`, `LANGFUSE_HOST` — if
      tracing agent traffic (see `.claude/hooks.json`).
- [ ] Any product-specific keys from `.env.example` that the deploy workflow
      needs at build time.

## 8. Claude skill paths

- [ ] Review `.claude/agents/*.md` frontmatter — adjust `maxTurns`, `model`,
      and `tools` per role if the product's budget is tighter or looser.
- [ ] Decide whether `.claude/hooks.json`'s PostToolUse Langfuse shape
      should be enabled on day one; if so, drop `scripts/langfuse-trace.sh`
      in and remove the `_disabled` flag.

## 9. Deploy workflow targets

- [ ] Walk `.github/workflows/deploy.yml` end-to-end and replace the
      commented-out Vercel / Fly.io examples with the real deploy steps.
- [ ] If the product deploys somewhere else entirely (Cloudflare Workers,
      Netlify, a custom K8s target), rewrite the jobs — don't keep the
      examples around as "we'll get to it".

## 10. Product-specific AGENTS.md content

- [ ] Fill in every `<!-- TODO: fill in. -->` block in `AGENTS.md`:
      overview, external docs, architecture, state machine, error codes,
      build order, secret table, type source of truth, make targets,
      handoff rules.
- [ ] Walk the universal anti-goals list and APPEND any product-specific
      ones; never remove the universal ones.
- [ ] Review Merge Safety Rules M-1..M-7 — keep the ones that apply, prune
      the ones that don't, add product-specific M-N if the product's
      incidents justify them.
- [ ] Update `docs/agent-registry.md` handoff packages and module boundaries
      with the real phase gates for this product.
