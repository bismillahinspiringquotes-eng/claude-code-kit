# Skills

Reusable Claude Code skills extracted from production discipline patterns. Each skill is a self-contained markdown file Claude can invoke on demand.

These skills are **opinionated** — they reflect the kit's specific philosophy (audit-existing-before-build, ship-verification, no-technical-leak in user copy). They are **stack-agnostic** — examples reference Supabase + Vercel + Next.js because that's the kit's recommended stack, but the underlying checks apply to any equivalent setup.

## How to use these skills

Two options:

**Option A — Copy into your own project's skill folder.** If your Claude Code setup loads skills from a known location, copy the relevant `.md` file there. The skill becomes invocable in your sessions.

**Option B — Reference inline.** Paste the body of a skill into your conversation when you want Claude to follow that specific protocol once.

Skills here are MIT-licensed; fork freely.

## Available skills

### Security

- **[security-audit-database.md](security-audit-database.md)** — 7-check audit of database security: multi-tenant data leaks, admin keys exposed to browser, missing auth guards, unencrypted personal data, public endpoints leaking sensitive fields, webhook handlers without signature checks, migrations dropping policies unsafely.
- **[security-audit-web-app.md](security-audit-web-app.md)** — 6-check audit of frontend/edge security: env var leaks, server code in client bundle, missing security headers, unsafe HTML rendering, open redirects, middleware bypass risks.

### SEO

- **[seo-audit-multilocale.md](seo-audit-multilocale.md)** — for sites serving multiple languages (Indonesian + English, etc.): hreflang correctness + reciprocity, sitemap language alternates, canonical URL consistency, server-rendered locale routing, structured data per locale, locale-aware redirects from old URLs.
- **[seo-content-audit.md](seo-content-audit.md)** — single-language on-page SEO: title tags, meta descriptions, heading hierarchy, image alt text, internal linking, structured data, sitemap + robots.txt correctness.

### Production discipline

- **[pre-deploy-3gate.md](pre-deploy-3gate.md)** — runs the 3-gate verification before claiming work is shipped (type check → build → curl live URL with content match). Refuses to call work shipped until all three pass. Operationalizes the SHIP VERIFICATION RULE.
- **[secret-rotation-walkthrough.md](secret-rotation-walkthrough.md)** — guided rotation cycle for any secret (Supabase key, payment provider, Cloudflare token, etc.) covering damage containment, system inventory, ordered deploy, revocation verification, postmortem.

### Voice / content

- **[voice-register-check.md](voice-register-check.md)** — audits user-facing strings for two failure modes: technical state leaking into user copy (`error`, `system`, `null`), and builder/dev jargon used where audience is non-engineer (`operator`, `PR`, `slug`, `asset`).

## Adding a new skill

See [../CONTRIBUTING.md](../CONTRIBUTING.md). Skills live in this directory as standalone `.md` files with frontmatter (`name`, `description`). The body should be the full protocol Claude follows when the skill is invoked.

Acceptance criteria for new skills:
- Solves a concrete problem the kit's audience hits in production
- Stack-agnostic by design (examples can reference Supabase/Vercel)
- Plain-language description (audience may be non-engineer)
- Ends with a clear deliverable (a report, a passing checklist, a concrete next action)
- Includes constraints / things-to-not-do, not just things-to-do
