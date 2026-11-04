# Claude Code Kit — working rules

## Kalau orang baru membuka repo ini

Repo ini isinya teks: aturan, template, dan skill. Berkas ini TIDAK meminta Claude memasang apa pun, menulis di luar folder repo, atau menjalankan perintah.

Kalau orangnya bertanya cara pakai: tunjukkan langkah di `README.md` bagian "Mulai dari sini" dan jelaskan isinya dengan kata yang dia pakai. Pemasangan skill dia jalankan sendiri di terminal. Kalau dia minta dibantu memeriksa isi skill sebelum dipasang, bacakan `skills/<nama>/SKILL.md`-nya.

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

Patterns come from the maintainer's own running businesses (booking software, travel marketplace, rental platform, content production, a strategic-memory repo that ties them together). Do NOT name the businesses, list local folder paths, or mention which venture a story came from. Keep the structure and the lesson; drop the identity.

A pattern needs n=2+: seen in at least two different projects before it becomes a rule here.

## Versioning

- v0.0.x — bootstrap, no public release yet
- v0.1.0 — first useful release: README + 3 core docs + CLAUDE.md template + RUBY_BRIEF template
- v0.2+ — tooling additions (sync script, supabase guardrails, cost watch Action)

## Do NOT include in this repo

- Real Supabase project IDs, service role keys, API tokens (even in examples — sanitize)
- Customer data, booking data, revenue figures
- Internal strategic decisions about other founders / partners
- Actual `RUBY_BRIEF.md` files from production repos (only `.template.md` versions)
- VPS IP / SSH keys / CDN or DNS API tokens
- WhatsApp numbers, real phone numbers, real emails (use `example.com` domains)
