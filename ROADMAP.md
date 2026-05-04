# Roadmap

What's planned. For what's already shipped, see [docs/INDEX.md](docs/INDEX.md) + [skills/INDEX.md](skills/INDEX.md).

This file exists so the docs index stays honest about what's actually there. "Planned" promises tend to rot — if a doc has been planned for 3 months without being written, it probably shouldn't be planned at all.

## Priority order (next ~5 to write)

1. **`docs/02-cross-repo-memory.md`** — sync! flow, brief-file producer/consumer protocol, UTC-Z timestamp lock. Source: existing canonical memory + accio definitions.
2. **`docs/11-recon-first.md`** — read codebase before writing code; lock spec in decision files. Source: established practice across 8+ repos.
3. **`docs/12-multi-tenant-rls.md`** — row-level security discipline + restore drill. Stack-agnostic, examples Supabase.
4. **`docs/13-per-operator-payments.md`** — per-operator payment routing vs marketplace-fee model. SEA reality (Xendit > Stripe for many SEA cases).
5. **`docs/20-secret-rotation.md`** — extract from `skills/secret-rotation-walkthrough.md` into doc form + recent destructive-action lesson.

## Methodology (planned)

- `docs/02-cross-repo-memory.md` — sync! flow, brief-file producer/consumer protocol, UTC-Z timestamp lock

## Patterns (planned)

- `docs/10-phase-ordering.md` — scaffold → schema/RLS → auth → UI → features → SEO → admin → DNS → polish
- `docs/11-recon-first.md` — read before code, lock spec in `decisions/*.md`
- `docs/12-multi-tenant-rls.md` — row-level security discipline + restore drill (examples: Supabase)
- `docs/13-per-operator-payments.md` — per-operator payment vs marketplace-fee (SEA reality)

## Runbooks / ops (planned)

- `docs/20-secret-rotation.md` — full rotation runbook (extracts + extends `skills/secret-rotation-walkthrough.md`)
- `docs/21-database-backup.md` — backup + restore drill (stack-agnostic; examples Postgres + R2)
- `docs/22-error-monitoring.md` — Sentry / GlitchTip / BetterStack patterns
- `docs/23-cost-monitoring.md` — billing-API → alert hooks (Supabase + Vercel patterns)
- `docs/24-dns-migration.md` — registrar move + nameserver swap with zero downtime

## Holding-company patterns / advanced (planned)

- `docs/30-cross-product-dependencies.md` — when product A consumes product B as a privileged channel (zero-fee inter-company)
- `docs/31-pt-entity-mapping.md` — Indonesian legal entity structure encoded as code rules
- `docs/32-content-distillation-loop.md` — founder edit pairs ↔ in-house AI agent ↔ monthly fine-tune loop

## What's NOT planned

These got considered + rejected, with reasoning:

- **Marketing playbook** — out of scope. Kit is methodology + tooling, not GTM.
- **Hiring guide** — kit is built FOR solo founders. Hiring patterns are different work.
- **Investor deck templates** — distinct concern from infrastructure layer.
- **AI agent autonomy patterns** — this kit explicitly takes the OPPOSITE thesis (infrastructure first, AI second). Autonomous-agent patterns belong in a different methodology.

## How to add to this roadmap

If you want to propose a planned doc that's not here:

1. Open an issue describing the pattern + which existing real-world incident or use case motivates it
2. Tag it `roadmap-proposal`
3. Discuss before drafting — see [CONTRIBUTING.md](CONTRIBUTING.md)

## Velocity reality

Foundation laid 2026-05-02. We don't ship to a fixed timeline — docs land when the underlying pattern is mature enough across multiple production repos that the lesson is durable. Single-venture patterns risk being incidental; the kit waits for n=2+.

If a planned doc has been on this roadmap for 3+ months without being written, it probably shouldn't be planned. Audit + delete is fine.
