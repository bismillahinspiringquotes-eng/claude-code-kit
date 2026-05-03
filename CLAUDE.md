# Claude Code Kit — onboarding + working rules

## CLAUDE: First-time visitor onboarding

If a user opens this repo and asks anything (or even just types "hi", "start", or "halo"), treat as an onboarding trigger. Don't just answer their literal question — first detect intent via ONE clarifying question. Match their language: respond in Indonesian if their first message was Indonesian, English if it was English.

**Indonesian opener** (use if user wrote in Indonesian):

> Halo! Lo di claude-code-kit. Mau gue bantuin:
> 1. Setup CLAUDE.md baru di project lo (new user)
> 2. Fix CLAUDE.md yang udah ada tapi bermasalah (existing user, frustrated)
> 3. Adopt specific pattern dari kit (browse docs)
> 4. Kontribusi pattern lo sendiri (PR)
>
> Atau: just browse aja dulu? Pilih (1-4) atau bilang "browse".

**English opener** (use if user wrote in English):

> Hey — you're in claude-code-kit. Want me to help with:
> 1. Set up a fresh CLAUDE.md in your own project (new user)
> 2. Fix an existing CLAUDE.md that isn't working well (frustrated user)
> 3. Adopt a specific pattern from the kit (browse docs)
> 4. Contribute a pattern you've developed (PR)
>
> Or just browsing? Pick (1-4) or say "browse".

**Based on answer, route to:**

- **(1) New user** → Walk them through `templates/minimal-claude-md.md` adapted to their stack. Ask 1-2 questions max (stack + role), then help them write + commit their first CLAUDE.md in their own project.
- **(2) Frustrated** → Ask: what's frustrating? Match the symptom to the right section in `docs/02-fixing-broken-claude-md.md`. Offer to read their existing CLAUDE.md (they paste the absolute path) and propose concrete edits.
- **(3) Pattern adopter** → List `docs/INDEX.md`, ask which doc interests them, then walk through the pattern + how to adapt it to their context.
- **(4) Contributor** → Route to `CONTRIBUTING.md`, discuss their pattern, help draft the PR (issue-first per the contributing guide).
- **"browse"** → Back off. Say "Cool, explore aja — kalau stuck tanya kapan aja" / "Cool, look around — ping me if you get stuck."

**Constraints on this directive:**

- Max 3 questions total before the user gets an actionable next step. Don't interrogate.
- Respect "browse" / "let me look first" / "ga usah dipandu" — back off immediately, no follow-up.
- If the user is clearly returning (working on an existing doc/template, references prior context, asks a repo-maintenance question), this directive does NOT apply — proceed normally per the rules below.
- **Voice when routing to English source docs while user is in Indonesian:** DO NOT verbatim-translate the doc's bullet structure into Indonesian. Verbatim translation produces stilted "translation Indonesian" (`yang lo selamatin`, `nempel di mana`, `ketiga ini = isi issue`) — that's exactly the voice the kit is built to AVOID. Instead: read the source doc for the gist, then have a normal conversation in casual Indonesian (gue/lo, natural phrasing). Ask questions as questions, not as numbered list items dictated by the source doc's structure. The directive's job is intent routing, not document recitation.

---

**BELOW: example CLAUDE.md content showing real production patterns** — the kit's own working rules, used as a live example of a disciplined CLAUDE.md.

---

# CLAUDE.md — Claude Code Kit (this repo)

**Repo type:** Open-source methodology + templates + tooling.
**Audience:** Solo founders + SEA/Indonesian developers + Claude Code adopters wanting opinionated infrastructure layer.
**Maintainer:** Ruby Perkasa.

## Hard rules when working in this repo

### Voice + audience
- README + public-facing docs: English primary (audience global), but use Indonesian examples authentically (gue/lo in founder-style snippets is a feature, not a bug).
- Internal docs (this CLAUDE.md, plan files): Indonesian casual OK with maintainer, English fine for technical names.
- Do NOT translate Indonesian voice rules into English in templates — that destroys the differentiator.
- User-facing copy in templates uses universal `aku/kamu` (warm-curator), NOT Jakarta slang (gue/lo). Ruby-Claude register ≠ Brand-User register.

### Substance > polish
- Every doc must include a real example from a production venture (anonymized if needed). Abstract patterns without example = useless for learners.
- Templates must be FORKABLE, not theoretical. Test by trying to fork and run.
- Don't add a new pattern unless extracted from at least 2 different ventures (single-venture patterns risk being incidental).

### Honesty
- This kit is NOT first-mover at most pillars. Owen Zanzal's Virtual Monorepo Pattern + claude-mem (46k+ stars) + WORKSPACE.md patterns predate parts of this. Acknowledge.
- Honest moat = composite + execution discipline + SEA-specific. Don't oversell.
- If a pattern competes with existing tooling (e.g., claude-mem), say so explicitly. Don't pretend they don't exist.

### Anti-patterns to avoid
- "Vibe coding to riches" framing — this kit is the OPPOSITE thesis (infrastructure first, AI second)
- Toy demos or hello-world content (audience already past that)
- Generic "AI is amazing" filler
- Closed-source dependencies in the recommended path (Infisical OK, Doppler not — break OS positioning)

## Working session protocol

### Read sequence at session start
1. `README.md` — positioning + current state
2. `CLAUDE.md` (this file) — rules
3. `docs/INDEX.md` if exists — what's shipped vs in-progress

### Update protocol
- New methodology pattern extracted → `docs/##-pattern-name.md` (numbered for read order)
- New template → `templates/[name].template.md` (always with `.template.` infix)
- New example → `examples/[venture-anonymous]/[topic].md`
- New tool → `tools/[tool-name]/` with own README

### Commit hygiene
- Commits should be substantive. "wip" commits OK during private dev, squash before merging to public.
- Use conventional commits: `docs: add CLAUDE.md pattern walkthrough`, `feat(tools): add supabase usage watch action`, etc.
- Don't auto-push (per Ruby's holding-wide rule). Only push when explicitly asked.

## Source material

This kit extracts patterns from these ventures (paths as of 2026-05-02):

- `~/Documents/scheduly/` — booking SaaS, multi-tenant, API-first. Source of: per-tenant API auth, cron worker patterns, integration with Suave/BR partners.
- `~/Documents/indahnesia-web/` — curated travel marketplace. Source of: Phase 0 publish API endpoint pattern, multi-locale content (en/id/zh), JSON-LD discipline.
- `~/Desktop/bajorental/` — rental platform. Source of: hreflang helper pattern, BR endpoint Phase 0 reference.
- `~/Documents/content-studio/` — Raris GM creative agency. Source of: persona registry, content distillation loop, voice fingerprint protocol.
- `~/Documents/ruby-assistant/` — strategic memory across all ventures. Source of: cross-repo memory protocol, sync! flow, accio! ritual, monyet! save trigger, MEMORY.md index, conversations/ archive.
- `~/Documents/leticialiveaboard/`, `~/Documents/bajolaundry/`, `~/Documents/karunggoni/`, `~/Documents/rumahkarunggoni/`, `~/Documents/suavetrip-laravel/` — secondary ventures, source of variant pattern observations.

When extracting: anonymize venture-specific data (slugs, ports, IDs) but preserve structure. Real production code reads as authentic; sanitized abstractions read as theoretical.

## What lives WHERE

- **This repo** = methodology + templates + tooling that's safe to publish.
- **`~/Documents/ruby-assistant/`** = private strategic memory + per-venture status. NOT published.
- **`~/Documents/ruby-assistant/projects/claude-code-kit*.md`** = original brainstorm + research + verify docs. Source for this repo. NOT republished verbatim.

When taking content from ruby-assistant private docs into this public repo:
1. Anonymize ventures if specific (replace "scheduly" with "[booking-saas]" or generalize)
2. Strip business-specific values (revenue numbers, customer names, internal politics)
3. Preserve patterns + reasoning (the WHY) — that's the value
4. Cross-reference back: "Pattern extracted from running [N] production ventures. See [examples/]."

## Versioning

- v0.0.x — bootstrap, no public release yet
- v0.1.0 — first useful release: README + 3 core docs + CLAUDE.md template + RUBY_BRIEF template
- v0.2+ — tooling additions (sync script, supabase guardrails, cost watch Action)

## Cross-Reference

- Original brainstorm: `~/Documents/ruby-assistant/projects/claude-code-kit.md`
- OS landscape research: `~/Documents/ruby-assistant/projects/claude-code-kit-OS-RESEARCH.md`
- Adversarial uniqueness verify: `~/Documents/ruby-assistant/projects/claude-code-kit-VERIFY.md`
- Codebase recon (extract source): `~/Documents/ruby-assistant/projects/claude-code-kit-RECON.md`

## Do NOT include in this repo

- Real Supabase project IDs, service role keys, API tokens (even in examples — sanitize)
- Customer data, booking data, revenue figures
- Internal strategic decisions about other founders / partners
- Actual `RUBY_BRIEF.md` files from production repos (only `.template.md` versions)
- VPS IP / SSH keys / Cloudflare API tokens
- WhatsApp numbers, real phone numbers, real emails (use `example.com` domains)


---

## Cross-Holding Memory (auto-loaded at session start)

@~/Documents/ruby-assistant/memory/CROSS_HOLDING_INDEX.md

If `@`-import is not resolved by your Claude Code version: explicitly read `~/Documents/ruby-assistant/memory/CROSS_HOLDING_INDEX.md` and the linked feedback files at session start. These are universal rules across all Ruby holding repos (voice, deploy verification, default-to-execute, forensic discipline). Apply alongside this repo's CLAUDE.md.

**Canonical source**: `~/Documents/ruby-assistant/memory/`. Never duplicate to this repo.
