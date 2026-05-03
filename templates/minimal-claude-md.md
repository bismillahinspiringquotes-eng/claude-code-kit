# CLAUDE.md — [Your Project Name]

> **How to use this file:**
> 1. Save this as a plain text file named `CLAUDE.md` (caps matter) in the root folder of your project.
> 2. Replace each `[bracketed placeholder]` with your real value, OR delete the line if it doesn't apply yet.
> 3. Restart your Claude Code session — Claude will load this file at session start.
>
> **If you're new:** start with just three sections — `Project`, `Voice rules`, `Hard rules`. Delete the rest until you need them. A short CLAUDE.md you actually maintain beats a long one you ignore.
>
> **Reference:** full pattern explanation in `claude-code-kit/docs/01-claude-md-rules.md`. Skip if you're just getting started.

---

## Project  *(start here — required)*
- **What:** [1-line description, e.g. "side project to learn Next.js" or "booking app for my mom's salon"]
- **Stack:** [e.g. "Next.js + Supabase" — or just "still figuring out" if you're new]
- **Localhost:** [port, optional — e.g. 3000]
- **Production URL:** [your URL — leave blank if not deployed yet]

## Voice rules  *(start here — required)*
- **With me** (this chat, when you're working with Claude): [your casual style — e.g. "Indonesian gue/lo" or "English, contractions OK, no formality"]
- **With end users** (UI copy, error messages, emails): [tone — e.g. "warm but professional" or "Indonesian aku/kamu, friendly"]
- Tech terms stay in English (API, migration, etc — never translated)

## Hard rules (NEVER violate)  *(start here — required)*
> Add 1-5 rules that, if Claude breaks them, would actually hurt your project.
> Each rule should have a one-line **Why:** so Claude understands the intent (not just the literal rule).
>
> Pick the intensity that matches where your project is — examples below scale from toy → side → production.

**Toy / learning project (1-2 rules is plenty):**
1. Don't `npm install` new packages without asking me first.
   **Why:** I want to learn what each dependency does, not have a black box.
2. Always explain code before writing it (1-2 sentences).
   **Why:** I'm here to learn, not to ship fast.

**Side project (2-3 rules):**
1. Run `npm test` before saying "done".
   **Why:** I've shipped broken builds twice trusting "looks good".
2. Never edit files outside `src/` without asking.
   **Why:** config files break in subtle ways I don't catch.

**Production (3-5 rules — these need real WHY from real incidents):**
1. Auth check must be the FIRST line of every API route.
   **Why:** any logic before the guard runs for unauthenticated callers.
2. Every POST/PATCH body validated with a schema — no raw `request.json()`.
   **Why:** untyped input bypasses every downstream check.
3. New database tables: enable row-level security + add `deleted_at` column in the same migration.
   **Why:** retrofitting either later requires a deploy window.

---

## *Sections below are OPTIONAL — add when you need them*

## Token discipline  *(add when Claude wastes tokens)*
- No preamble. No "Great question!". No filler.
- Don't restate plan after I say "go" / "execute" / "ya".
- Max 1 question per response.
- Reports as bullets/tables — prose only when asked.

## Trigger words  *(add when you have repeating session rituals)*
- `start` → read this file + recent session state, brief status report
- `accio!` → end-of-session ritual: update progress notes + save
- `monyet!` → save strategic decisions to a DECISIONS file
- `catat itu` → save what we just discussed to a memory file

> Why weird words like `monyet!`: they don't collide with normal conversation, so Claude knows they're commands.

## File map  *(add when your project has 3+ doc files)*
- `[file]` → [what it owns]
- `[dir/]` → [what's in it]
- **Rule:** 1 topic = 1 file. Don't update 2 files for 1 thing.

## Pre-commit checks  *(add once you have a build / test workflow)*
- [build command, e.g. `npm run build`] must pass
- [type check command, e.g. `tsc --noEmit`] must pass
- No `console.log` in production code paths
- No hardcoded secrets / API keys

## How to extend this file
- Hit a problem twice? → add a rule + one-line **Why:** capturing the cause
- Every 2 weeks: re-read your CLAUDE.md. Delete rules that haven't been triggered.
- Read `claude-code-kit/docs/01-claude-md-rules.md` when you want the full pattern reasoning
- Read `claude-code-kit/docs/02-fixing-broken-claude-md.md` if Claude starts misbehaving despite this file
