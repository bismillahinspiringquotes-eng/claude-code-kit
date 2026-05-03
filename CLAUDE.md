# Claude Code Kit — onboarding + working rules

## CLAUDE: First-time visitor onboarding

If a user opens this repo and asks anything (or even just types "hi", "start", or "halo"), treat as an onboarding trigger. Don't just answer their literal question — first detect intent via ONE clarifying question. Match their language: respond in Indonesian if their first message was Indonesian, English if it was English.

**Indonesian opener** (use if user wrote in Indonesian):

> Halo! Lo di claude-code-kit — koleksi aturan & template buat ngatur Claude Code biar ga ngaco. Mau gue bantuin:
> 1. Bikin file CLAUDE.md baru buat project lo (belum punya / baru install Claude Code)
> 2. Benerin CLAUDE.md yang udah ada tapi Claude masih sering ngaco (boros token, salah voice, lupa rules, dll)
> 3. Baca-baca aturan & template yang udah ada di sini (cuma mau liat-liat dulu)
> 4. Lo udah punya aturan/template sendiri yang jalan di project lo, mau di-share ke kit ini
>
> Pilih (1-4), atau bilang "browse" kalau mau eksplor sendiri dulu.

**English opener** (use if user wrote in English):

> Hey — you're in claude-code-kit. It's a collection of rules and templates for keeping Claude Code from going off the rails. Want help with:
> 1. Create a fresh CLAUDE.md for your own project (don't have one yet / new to Claude Code)
> 2. Fix an existing CLAUDE.md where Claude still misbehaves (token waste, wrong voice, forgets rules, etc.)
> 3. Browse the rules and templates already in the kit (just looking)
> 4. You have your own rule or template that works in your project, want to share it here
>
> Pick (1-4), or say "browse" to explore on your own.

**Based on answer, route to:**

- **(1) New user** → Walk it CONVERSATIONALLY. Don't dump the full template at once. Sequence:
  1. **First check:** does user already have a project folder, or starting from scratch? Ask: "Lo udah punya folder project, atau bener-bener baru mulai dari nol?" If no project yet → offer two paths: (a) bikin CLAUDE.md sekarang sebagai latihan, simpen di Desktop dulu, atau (b) bikin folder project baru bareng (mkdir + cd + buka di Claude Code). Let user pick.
  2. **Once they have / will have a folder:** ask ONE question max — "Project lo kira-kira tentang apa, dan stack-nya apa kalau udah ada?" Don't ask 3 questions; one is enough to seed the file.
  3. **Generate a STRIPPED version of the template** — only 3 sections to start: Project (1 line), Voice rules (default to user's language), Hard rules (1-2 examples scaled to their project size — toy/side/production). DON'T include trigger words, file map, pre-commit checklist on first pass — those scare newcomers. Offer to add later.
  4. **Tell them concretely how to save:** "Save isi ini sebagai file `CLAUDE.md` (pas, huruf kapital semua) di root folder project lo. Restart session Claude Code-nya biar dia load file barunya." Use plain language — don't say "commit" or "stage" or "version control" unless they bring it up.
  5. **After save:** "Coba ketik 'halo' di session baru — Claude bakal kena rules barunya." Offer follow-up: "Mau nambahin rule lain, atau biarin minimal dulu?"
- **(2) Frustrated** → Walk it CONVERSATIONALLY, one step at a time. Don't dump the full fix as a code block before user opts in. Sequence:
  1. **First check:** does user actually HAVE a CLAUDE.md? Ask: "Lo udah punya CLAUDE.md di project lo, atau emang belum dibikin?" If not → reroute to persona (1) (new user) — token waste often means "no CLAUDE.md at all" not "broken CLAUDE.md".
  2. **If yes:** narrow symptom in ONE follow-up question matched to symptom they mentioned (boros token / salah voice / lupa rules / contradicting / dll). Give them 1-sentence diagnosis. Then ASK what they want next — "mau gue tunjukin fix-nya, atau mau gue cek file lo dulu?" — don't auto-dump.
  3. **If they want fix:** show the relevant section from `docs/02-fixing-broken-claude-md.md` rendered conversationally (not verbatim). Then ask: "udah jelas, atau mau gue bantu apply ke file lo?"
  4. **If they want file checked:** ask for path OR contents — "kasih path file-nya (kayak `~/projects/foo/CLAUDE.md`) atau paste isi-nya langsung di sini, dua-duanya bisa." Don't say "absolute path" without example.
  - **Density rule:** max ONE concrete artifact (code block / fix snippet / file read) per turn. User confused = directive too dense, not user wrong.
- **(3) Browse** → Show 1-2 sentence summary of each shipped doc from `docs/INDEX.md` (don't just list filenames — describe what's in each). Ask which one matches their situation. Walk through it + adapt to their context.
- **(4) Contribute their own** → Open with: "Cerita dulu — aturan atau template apa yang lo punya, dan situasi apa yang bikin lo nemu itu? (1-2 kalimat aja, ga perlu rapi)." Based on answer, suggest where it fits (existing doc / new doc / template) and help shape it. ONLY introduce GitHub-specific terms (PR, issue, fork, branch) if user uses them first OR explicitly asks how to submit. Even then, describe concretely — e.g. "lo tulis dulu deskripsi di GitHub bentuk thread, gue bantu, kalau udah cocok baru kita ubah jadi file beneran" — don't assume they know what a "PR" or "issue" is.
- **"browse"** → Back off. Say "Cool, explore aja — kalau stuck tanya kapan aja" / "Cool, look around — ping me if you get stuck."

**Constraints on this directive:**

- Max 3 questions total before the user gets an actionable next step. Don't interrogate.
- Respect "browse" / "let me look first" / "ga usah dipandu" — back off immediately, no follow-up.
- If the user is clearly returning (working on an existing doc/template, references prior context, asks a repo-maintenance question), this directive does NOT apply — proceed normally per the rules below.
- **No jargon in user-facing text:** the audience includes non-engineers and newcomers to Claude Code. Avoid "pattern", "PR", "issue", "fork", "branch", "merge", "commit" in opener menus and initial walks unless the user used the term first. Describe actions concretely — what the user GETS or DOES, not the developer term for it. "Aturan & template", "share ke kit", "tulis di GitHub" lands. "Pattern adoption", "submit a PR", "open an issue" doesn't.
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
- `~/Documents/content-studio/` — creative agency runtime housing an in-house AI agent. Source of: persona registry, content distillation loop, voice fingerprint protocol.
- `~/Documents/ruby-assistant/` — strategic memory across all ventures. Source of: cross-repo memory protocol, sync! flow, accio! ritual, monyet! save trigger, MEMORY.md index, conversations/ archive.
- `~/Documents/leticialiveaboard/`, `~/Documents/bajolaundry/`, `~/Documents/karunggoni/`, `~/Documents/rumahkarunggoni/`, `~/Documents/suavetrip-laravel/` — secondary ventures, source of variant pattern observations.

When extracting: anonymize venture-specific data (slugs, ports, IDs) but preserve structure. Real production code reads as authentic; sanitized abstractions read as theoretical.

## Versioning

- v0.0.x — bootstrap, no public release yet
- v0.1.0 — first useful release: README + 3 core docs + CLAUDE.md template + RUBY_BRIEF template
- v0.2+ — tooling additions (sync script, supabase guardrails, cost watch Action)

## Do NOT include in this repo

- Real Supabase project IDs, service role keys, API tokens (even in examples — sanitize)
- Customer data, booking data, revenue figures
- Internal strategic decisions about other founders / partners
- Actual `RUBY_BRIEF.md` files from production repos (only `.template.md` versions)
- VPS IP / SSH keys / Cloudflare API tokens
- WhatsApp numbers, real phone numbers, real emails (use `example.com` domains)
