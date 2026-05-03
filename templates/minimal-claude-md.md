# CLAUDE.md — [Your Project Name]

> Minimal CLAUDE.md skeleton from claude-code-kit.
> Customize each section for your project, then delete this comment block.
> Full pattern explanation: claude-code-kit/docs/01-claude-md-rules.md

## Project
- **What:** [1-line description, e.g. "Booking SaaS for Indonesian travel operators"]
- **Stack:** [e.g. Next.js 14 + Supabase + Vercel]
- **Localhost:** [port, e.g. 3000]
- **Production URL:** [your URL]

## Voice rules
- With me (founder, this chat): [your casual style — gue/lo, English contractions, etc.]
- With end users (UI copy, emails): [register — universal aku/kamu, professional but warm, etc.]
- Tech identifiers stay English (API, migration, RLS — never translated)

## Token discipline
- No preamble. No "Great question!". No filler.
- Don't restate plan after I say "go" / "execute" / "ya".
- Max 1 question per response.
- Reports as bullets/tables — prose only when asked.

## Trigger words
- `start` → read this file + recent session state, brief status report
- `accio!` → end-of-session: update progress files + commit
- `monyet!` → save strategic decisions to DECISIONS.md
- `catat itu` → save current discussion to memory file

## Hard rules (NEVER violate)
> Add 3-5 rules specific to your stack. Each rule needs a one-line **Why:**
> so Claude can extend it correctly to edge cases. Examples:

1. Auth check must be FIRST line of every API route.
   **Why:** any logic before the guard is reachable by unauthenticated callers.
2. Every POST/PATCH body validated with Zod — no raw `request.json()`.
   **Why:** untyped input bypasses every downstream invariant.
3. New tables: RLS enabled + `deleted_at` column in same migration.
   **Why:** retrofitting either later requires a deploy window.

## File map (where things live)
- `[file]` → [what it owns]
- `[dir/]` → [what's in it]
- **Rule:** 1 topic = 1 file. Don't update 2 files for 1 thing.

## Pre-commit (automatic)
- [build command] must pass
- [type check command] must pass
- No console.log in production paths
- No hardcoded secrets

## How to extend this file
- Every production incident → add a rule + one-line WHY
- Audit every 2 weeks → delete rules that never trigger
- Read `claude-code-kit/docs/01-claude-md-rules.md` for full pattern
- Read `claude-code-kit/docs/02-fixing-broken-claude-md.md` if Claude misbehaves
