# <product> — Agent Registry

| Agent role | Scope | Input | Output |
|---|---|---|---|
| developer | Full-stack implementation | tasks (by phase) | Working code, passing tests |
| code-reviewer | Per-milestone fresh-context review | Git diff | Review findings |
| tester | QA (visual + functional + mobile) | Codebase | QA report |
| security | Security pass (Tier 4) | Codebase | Security findings |

## Handoff packages

<!-- TODO: fill in the gate handoffs for this product. Example shape: -->

- **Phase 0 → Phase 1**: Spike results + Gate 0 sign-off
- **Phase 1 → Phase 2**: `make dev` working, stub job completes via UI
- **Phase 2 → Phase 3**: Real end-to-end path runs under budget
- **Phase 3 → Phase 4**: Friends can use it without help
- **Phase 4 → QA**: All phases built, monitoring configured

## Module boundaries (no cross-module coupling except via types-contract)

<!-- TODO: declare the firewall between modules for this product. Example: -->

- `web-ui` and `worker-core` NEVER share code directly — all contracts go through `packages/types`
- The core pipeline is a pure function: `(input) → output`
- `api-routes` never calls the core pipeline directly — always via a queue

## Agent instructions

Each agent receives:

1. This registry (agent-registry.md)
2. AGENTS.md (root context)
3. The relevant phase tasks
4. The prior phase handoff package

Code-reviewer fires at each milestone boundary with a fresh context — it reads the
git diff, not the session history. Security fires once before cutover and on
demand for billing / auth / RLS surfaces.

The behavioural definitions for each role live in `.claude/agents/<role>.md`.
