# Philosophy

## The thesis

**AI tanpa infrastruktur = mainan mahal.**

Watched 20+ "AI millionaire" podcasts. Pattern: every "one-person billion dollar company" story skips the 3 months of unsexy infrastructure setup before AI-driven revenue showed up.

The infrastructure is the moat.

## Three founder values that this kit encodes

These are personal — Ruby Perkasa's. Take them or don't, but they explain every design decision in this kit.

### 1. Vibe-coding moat

> "Semua orang bisa vibe coding. Tapi ga semua orang punya business logic."

Anyone can prompt an LLM. Few have the business context to know what to prompt. The moat = combining business context (which AI can't generate) with execution velocity (which AI accelerates).

This kit assumes you already have business context. It just gives you the velocity layer.

### 2. Scratch-your-own-itch

> "Ya yang gue buat pasti yang gue rasain painnya."

Build for the pain you actually feel. Every pattern in this kit comes from a real production failure — not a hypothetical. When you read a runbook here, it exists because something broke and someone wasted hours.

Examples (real incidents, paraphrased):
- "Phase 7.5 silent deploy fail" — TypeScript passed, Vercel build failed silently due to lint-as-error in production mode. Prod served stale code for 24 hours. Spawned the 3-gate verification rule.
- "Article #5 DB-orphan" — MD file written + commit pushed, claimed shipped, page 404'd because /learn route reads from DB not filesystem. Spawned the SHIP VERIFICATION rule (curl the URL, not "deploy ran").
- "Webhook subscription disappeared" — 8/8 rehearsal pass morning, ZERO rows by afternoon. Spawned cross-session forensic discipline.

Every doc in this kit traces back to an incident. If a doc seems abstract, an incident is missing — file an issue.

### 3. Mandiri

> "Mandiri aja, ga bergantung sama AI orang."

Self-reliance over dependency. This kit doesn't lock you into a SaaS, an agency, or someone else's framework. Every recommended tool is open-source or has a self-hosted path.

Concrete consequences:
- Recommended path: self-hostable (Infisical for secrets, GlitchTip / Sentry self-hosted for monitoring, pg-r2-backup for backups)
- Avoid: closed-source SaaS lock-in (Doppler, SimpleBackups, etc) in default recommendations
- Templates work offline-first where possible

## Why this kit exists (vs alternatives)

| Alternative | What it gives | What it lacks |
|---|---|---|
| Yet another Next.js starter | Boilerplate code | Methodology, memory protocol, ops runbooks |
| claude-mem (46k stars) | Per-repo memory | Cross-repo aggregation, holding-company semantics |
| Owen Zanzal's Virtual Monorepo Pattern | 35-repo orchestration | SEA-specific patterns, Indonesian voice |
| Spec-Kit + variants | `/specify` + `/plan` vocabulary | Memory protocol, deploy verification |
| Maker template ecosystems | Component library | Founder-voice register, multi-venture cognitive load |

This kit's specific niche:

> The only public methodology that integrates cross-repo memory + SEA operational reality + holding-company semantics + Bahasa-first founder voice into one Claude Code system.

Composite + execution discipline. Not first-mover at any single mechanism.

## Anti-thesis

This kit explicitly rejects:

- **"AI replaces developers"** — AI accelerates developers. The kit assumes the user is doing the thinking.
- **"Vibe-coding to riches"** — vibe-coding is fine for prototypes. Production needs the boring infrastructure layer.
- **"One prompt and you're done"** — production systems are 50+ prompts, dozens of CLAUDE.md updates, and a memory protocol that survives context resets.
- **"AI agents will run my company autonomously"** — they won't, not yet, and treating them like they do = production incidents.

If you came here looking for the magic shortcut, this is the wrong kit. The kit IS the boring infrastructure layer. The magic is what you build on top.
