# Docs Index

What's actually shipped right now. For planned/upcoming items see [ROADMAP.md](../ROADMAP.md).

Read order is the numbered prefix. Skip around if you know what you're looking for.

## Methodology

- [00-philosophy.md](00-philosophy.md) — why this kit exists, founder values, anti-thesis
- [01-claude-md-rules.md](01-claude-md-rules.md) — the actual CLAUDE.md ruleset running across 8 production repos (Indonesian primary)
- [02-fixing-broken-claude-md.md](02-fixing-broken-claude-md.md) — diagnostic for when Claude misbehaves: 6 common failure modes + specific fixes
- [03-session-rituals.md](03-session-rituals.md) — trigger word system: `start` / `accio!` / `monyet!` / `catat itu` / `sync!`
- [04-deploy-verification.md](04-deploy-verification.md) — the 3-gate rule (type-check → build → curl-content-match)
- [05-default-to-execute.md](05-default-to-execute.md) — Claude executes via tools, doesn't delegate; silence after execute
- [06-audit-existing-before-build.md](06-audit-existing-before-build.md) — verify what exists before building new (reuse > rebuild)
- [07-destructive-action-discipline.md](07-destructive-action-discipline.md) — verify command behavior before suggesting state-mutating ops
- [08-research-paper-full-read.md](08-research-paper-full-read.md) — read figures + appendix; never trust AI-summarized text alone
- [09-no-domain-jargon-user-facing.md](09-no-domain-jargon-user-facing.md) — builder vocabulary leak in user-facing copy
- [10-no-technical-leak-in-user-copy.md](10-no-technical-leak-in-user-copy.md) — system state leak in user-facing copy
- [14-bahasa-first-voice.md](14-bahasa-first-voice.md) — Indonesian-first voice + 6 register modes (founder ↔ user)

## Skills

- [../skills/README.md](../skills/README.md) — ready-to-install Claude Code skills (one folder = one skill). Start with the 4 basic ones: `anti-ai-slop`, `pakai-alat-dulu`, `bukti-sebelum-klaim`, `tanya-seperlunya`. Plus: security audits ×2, SEO audits ×2, pre-deploy verification, secret rotation, voice register check.

## Templates

- [../templates/CLAUDE.md.template](../templates/CLAUDE.md.template) — full reference CLAUDE.md skeleton
- [../templates/minimal-claude-md.md](../templates/minimal-claude-md.md) — newcomer-friendly minimal CLAUDE.md (3 required sections, optional sections marked)
- [../templates/RUBY_BRIEF.md.template](../templates/RUBY_BRIEF.md.template) — cross-repo state brief (producer side)

## Status

- ✅ Methodology docs: 12 shipped (00-10, 14)
- ✅ Skills: 11 shipped (4 basic + 7 specialized), Claude Code folder format
- ✅ Templates: 3 shipped (bootstrap)
- See [ROADMAP.md](../ROADMAP.md) for planned docs

v0.1.0 (2026-11-04): beginner path in README, skills in installable folder format, 4 basic skills. Foundation laid 2026-05-02.

## How to contribute

See [../CONTRIBUTING.md](../CONTRIBUTING.md). PRs welcome.

Punya aturan atau skill sendiri yang terbukti jalan? Ceritakan lewat GitHub repo ini, atau salin dan sesuaikan untuk proyek lo.
