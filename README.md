# Claude Code Kit

**Opinionated methodology for solo founders shipping multiple ventures with Claude Code.**

> The boring infrastructure part. The bit that doesn't make YouTube thumbnails but actually makes you money.

## What this is

Methodology + templates + tooling extracted from running 8+ active ventures under one holding (PT Adikarya Wisata Indah Nesia) with Claude Code as the primary development partner. SEA payment realities, Indonesian dev voice, cross-repo memory protocol, deploy verification gates.

Not a Next.js starter. Not a "1 prompt jadi miliarder" guide. The plumbing that millionaire-AI podcasts skip.

## What this isn't

- A toy demo or hello-world
- "Use Claude Code to vibe-code your way to riches"
- Yet another Next.js + Supabase boilerplate (it sits *above* boilerplates, starter-agnostic)
- A complete framework — it's opinionated patterns + working templates, not a black box

## Why exist

Watched 20+ AI-millionaire podcasts. Pattern: every "one-person billion dollar company" story skips the 3 months of unsexy infrastructure setup before the AI-driven revenue showed up. That infrastructure is the actual moat.

This kit extracts that infrastructure layer:
- CLAUDE.md per-repo discipline
- Cross-session memory (RUBY_BRIEF + sync! protocol, UTC-Z timestamp lock)
- Deploy verification gates (3-gate: tsc → build → curl)
- SEA payment patterns (per-operator Xendit, NOT marketplace-fee Stripe)
- Indonesian-first dev voice rules (gue/lo with founder, aku/kamu with end users)
- Holding-company semantics (1 founder / N companies / 1 PT)

## Honest positioning

This is not "the only way" or "uncontested." Cross-repo memory has prior art (Owen Zanzal's Virtual Monorepo Pattern, claude-mem 46k+ stars, WORKSPACE.md patterns). Founder-rules-as-code is a crowded genre.

What's specific to this kit:

> The only public methodology that integrates cross-repo memory + SEA operational reality + holding-company semantics + Bahasa-first founder voice into one Claude Code system.

Composite + execution discipline + first-mover in Indonesian dev community.

## What's inside (current state)

```
claude-code-kit/
├── README.md            ← you are here
├── CLAUDE.md            ← rules when working IN this repo
├── LICENSE              ← MIT
├── docs/                ← methodology docs (philosophy → patterns → runbooks)
├── templates/           ← fork-and-fill templates (CLAUDE.md, RUBY_BRIEF.md, sync flow)
├── examples/            ← real anonymized examples from production ventures
├── tools/               ← differentiator tools (sync script, supabase guardrails, cost watch)
└── .github/             ← reusable Actions workflows
```

Status: **bootstrap stage.** Foundation laid 2026-05-02. Iterating in public.

## Quickstart (when ready)

Coming. First version target: docs/00-philosophy.md → docs/01-claude-md-pattern.md → templates/CLAUDE.md.example.

## Who this is for

- Solo founders running multiple SaaS / ventures who need to keep state coherent across repos
- Indonesian/SEA developers wanting a localized methodology baseline
- Engineers tired of "yet another Next.js starter" content who want the orchestration layer above
- Non-engineers who learned vibe-coding but hit the wall when shipping production reality

## Who this is NOT for

- People wanting AI to do everything autonomously (this requires founder discipline)
- Solo-product-only founders (cross-repo patterns are overhead for 1 repo)
- Teams (this is built for one human's cognitive load — team variants exist elsewhere)

## License

MIT. Fork freely. Improve back if useful.

## Author

Ruby Perkasa — solo founder, 8+ active ventures, Bandung + Labuan Bajo.
