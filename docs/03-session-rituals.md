# Session rituals: trigger words

> Short, deliberately-weird keywords that act as commands distinct from natural conversation. Each trigger has a defined ritual. The friction is the feature — `monyet!` cannot be mistaken for casual chat.

## Why trigger words exist

Natural conversation is ambiguous. "Save this" could mean save the file we're editing, save this idea for later, or just remember it for the rest of the session. A trigger word like `monyet!` is unambiguous — it's a command, signaled by the very fact that no one says `monyet!` in conversation.

Trigger words solve three problems:
1. Distinguishing commands from chat
2. Avoiding repeating the full ritual every session
3. Making rituals consistent across multiple repos

## Recommended starter set

Adapt names + content to your project. The 5 below are extracted from running multiple production repos.

### `start`

Read CLAUDE.md → README → progress notes → session state. Brief status: "Active: [X]. Last: [Y]. Next: [Z]. Blockers: [N]."

Variant: `start [topic]` → also load topic-specific files (e.g. `start payment` → also reads `docs/PAYMENT_PLAN.md`).

### `accio!`

End-of-session ritual. Per project, define what this includes — typically:
1. Update progress file (mark completed items)
2. Update session file (next actions, blockers)
3. Update cross-repo brief if you have one
4. Bump version
5. Single commit, single push (or save without push if your rule is "never auto-push")

Reply: "Saved. Next: [one line]." Don't recap what was done.

### `monyet!`

Mid-session signal: save strategic decisions to `DECISIONS.md` (or equivalent). Distinct from `accio!` — `monyet!` is reactive (something just got decided), `accio!` is end-of-session.

### `catat itu`

"Note that down." Save whatever was just discussed to the most relevant memory file. Auto-detect destination: decision → DECISIONS, rule → CLAUDE.md, voice rule → voice memory.

### `sync!`

For multi-repo setups: pull cross-repo state. The opposite of `accio!` (which writes brief), `sync!` reads briefs from all your repos and updates the central memory. See ROADMAP `02-cross-repo-memory.md`.

## Why pick weird words

`monyet!` (Indonesian, informal/playful for "monkey") is intentionally weird. `accio!` is from Harry Potter. `catat itu` is the imperative form, slightly more aggressive than casual.

The friction is the feature. If you say "save this" and Claude isn't sure whether you meant the command or just "keep this in mind" — you have to clarify. With `monyet!`, no ambiguity.

You don't have to use these specific words. Pick your own — but pick deliberately weird ones, not "save" / "commit" / "done".

## Auto-detect rules (without explicit trigger)

Some patterns should auto-save without needing a trigger:

- **Decision detected** ("ga jadi build X", "X gratis dulu", "pilih A bukan B") → DECISIONS.md
- **Rule detected** ("jangan X", "always Y", "never Z") → CLAUDE.md
- **Priority change** ("X lebih penting", "deprioritize Y") → session file priority queue
- **Next-session intent** ("besok lanjut X") → session file
- **Feedback detected** ("parser jangan brute force", "trace dulu") → memory file

Auto-detect reduces the need for manual triggers — Claude saves continuously, the trigger words are for batched / explicit checkpoints.

## Cross-repo trigger consistency

If you have multiple repos, use the SAME trigger words across all of them. Otherwise muscle memory breaks.

The accio ritual contents may vary per-repo (different files to update), but the trigger word stays consistent.

## Constraints

- Don't add a trigger word for every possible ritual. 5-7 max. Beyond that, you forget which is which.
- Don't make trigger words English-words-with-different-meaning ("save", "commit", "publish", "finish") — they collide with intent.
- Document each trigger in your CLAUDE.md. Future-you needs the reference.
- Triggers are for Ruby/Founder ↔ Claude — don't expose them to end users.

## Related

- [`templates/CLAUDE.md.template`](../templates/CLAUDE.md.template) — has trigger word section
- [`templates/minimal-claude-md.md`](../templates/minimal-claude-md.md) — shows triggers as optional add-on (not required day 1)
- [`01-claude-md-rules.md`](01-claude-md-rules.md) — covers WHY weird words don't collide with conversation
- ROADMAP `02-cross-repo-memory.md` — full sync! / accio! producer-consumer protocol (planned)
