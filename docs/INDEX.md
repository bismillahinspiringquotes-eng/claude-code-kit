# Docs Index

Read order is the numbered prefix. Skip around if you know what you're looking for.

## Methodology

- [00-philosophy.md](00-philosophy.md) — why this kit exists, founder values, anti-thesis
- `01-claude-md-pattern.md` — *(planned)* — the per-repo CLAUDE.md as protocol agreement
- `02-cross-repo-memory.md` — *(planned)* — sync! flow, RUBY_BRIEF producer/consumer, UTC-Z timestamp lock
- `03-session-rituals.md` — *(planned)* — accio! / monyet! / catat itu / sync! / start triggers
- `04-deploy-verification.md` — *(planned)* — 3-gate (tsc → build → curl) with real incident anatomy
- `05-default-to-execute.md` — *(planned)* — Claude executes, doesn't delegate; the silence-unless-asked rule

## Patterns

- `10-phase-ordering.md` — *(planned)* — scaffold → schema/RLS → auth → UI → features → SEO → admin → DNS → polish
- `11-recon-first.md` — *(planned)* — read before code, lock spec in decisions/*.md
- `12-multi-tenant-rls.md` — *(planned)* — Supabase RLS discipline + restore drill
- `13-per-operator-payments.md` — *(planned)* — Xendit per-operator vs marketplace-fee Stripe (SEA reality)
- `14-bahasa-first-voice.md` — *(planned)* — gue/lo with founder, aku/kamu with users; register modes

## Runbooks (ops)

- `20-secret-rotation.md` — *(planned)* — Supabase API key rotation, Vercel env sync, VPS cron worker restart
- `21-supabase-backup.md` — *(planned)* — pg-r2-backup + restore drill
- `22-error-monitoring.md` — *(planned)* — Sentry SDK + BetterStack DSN
- `23-cost-monitoring.md` — *(planned)* — Supabase Management API + Vercel API → Discord alert
- `24-dns-migration.md` — *(planned)* — registrar move, nameserver swap, no downtime

## Holding-company patterns (advanced)

- `30-cross-product-dependencies.md` — *(planned)* — when product A consumes product B as 0% privileged channel
- `31-pt-entity-mapping.md` — *(planned)* — Indonesian legal structure encoded as code rules
- `32-content-distillation-loop.md` — *(planned)* — Raris GM ↔ Ruby edit pairs ↔ monthly LoRA fine-tune

## Status

- ✅ Shipped: 00-philosophy.md, this INDEX.md, README.md, CLAUDE.md, LICENSE
- 🟡 Templates: bootstrap (CLAUDE.md.template, RUBY_BRIEF.md.template — coming next)
- 🔴 All other docs: planned, not written

This is a **bootstrap-stage repo.** Foundation laid 2026-05-02. Iterating in public.

## How to contribute

Issues + PRs welcome once v0.1.0 ships. For now, fork freely.

If you want to surface a pattern from your own ventures: open a discussion (when GitHub Discussions are enabled) or just fork and adapt.
