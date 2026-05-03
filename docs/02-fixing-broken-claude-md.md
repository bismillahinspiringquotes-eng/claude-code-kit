# Fixing a CLAUDE.md That Isn't Working

> If you're frustrated with how Claude behaves in your repo, the CLAUDE.md is usually the problem — not the model. This doc walks through the 6 most common failure modes and the specific fixes.

## Quick diagnosis

Match your symptom to a failure mode:

| Symptom | Failure mode | Fix |
|---|---|---|
| Claude wastes tokens on preamble + restates plan after every step | No token discipline | §1 |
| Rules contradict each other; Claude picks one inconsistently | Contradictory rules | §2 |
| Claude follows the literal rule but the result still feels wrong | Rules without WHY | §3 |
| Claude treats "save this" / "go" as conversation, not command | No trigger words | §4 |
| Claude defaults to formal/corporate voice even though you wrote "casual" | Voice rules invisible | §5 |
| CLAUDE.md is 600+ lines, Claude misses important rules | Bloat — important rules buried | §6 |

## §1. No token discipline

**Symptom:** Claude opens with "Great question! Let me think about this..." Repeats your request back. Restates plan after every step. Asks 3 confirmation questions in one response.

**Fix** — add this near the top of CLAUDE.md:

```
## Token discipline
- Cut 60% output fat. Meaning intact.
- No preamble. No "Great question!". No filler.
- Don't explain before executing — just do it.
- Max 1 confirmation per task. Not per step.
- Max 1 question per response.
- When user says "go" / "execute" / "ya" → execute, don't restate plan.
```

The pairing matters. Confirmation rules WITHOUT the execute-on-greenlight rule make Claude over-cautious — you'll get "Mau gue lanjut?" 3x per task. Both halves required.

## §2. Contradictory rules

**Symptom:** rule on line 50 says "always use server actions for mutations". Rule on line 220 says "always use API routes for mutations". Claude picks one inconsistently.

**Fix:**

1. Grep CLAUDE.md for `always` and `never`. List every occurrence.
2. For each pair touching the same domain (auth, validation, routing, styling), check for contradiction.
3. Resolve to ONE canonical rule. Delete or scope the loser.
4. If both are conditionally needed, write the condition explicitly: *"Use server actions for mutations from RSC. Use API routes for external webhook receivers."*

Rule of thumb: if you can't tell from the rule WHEN it applies, neither can Claude.

## §3. Rules without WHY

**Symptom:** rule says `never use select("*")`. Claude follows it — then writes `SELECT col1, col2, col3, col4, col5, col6` (every column). The intent was *"explicit columns to prevent future column leak"*; without that WHY, Claude optimized for the literal rule.

**Fix** — every hard rule gets a `Why:`:

```
- NEVER use select("*") — always explicit column lists
  Why: prevents leaking new sensitive columns added later (a future PII column
  would auto-flow to public endpoint without code change otherwise).
```

Now Claude can extend the rule correctly to edge cases. New column added → automatically excluded from public response.

## §4. No trigger words

**Symptom:** you say "save this" — Claude says "OK" but doesn't actually save to a file. You say "let's pause here" — Claude waits politely instead of running an end-of-session ritual.

**Fix** — define explicit trigger words:

```
## Triggers
- `start` → read CLAUDE.md + SESSION.md, lapor status singkat
- `accio!` → end-of-session: update PROGRESS, version bump, commit
- `monyet!` → mid-session: save strategic decisions to DECISIONS.md
- `catat itu` → save current discussion to memory file
```

Why weird words like `accio!` / `monyet!` — they don't collide with natural conversation. "Save this" is ambiguous; `monyet!` is clearly a command. Friction = signal.

## §5. Voice rules invisible

**Symptom:** you wrote "casual Indonesian, gue/lo with me". Claude still uses "saya/anda" half the time, especially when generating UI copy.

**Fix** — split the register explicitly into TWO modes:

```
## Voice register (two modes — never confuse)
- With me (this chat): gue/lo, casual, no formality
- With end users (UI copy, emails, error messages): aku/kamu, warm but neutral
- Tech identifiers stay English (API route, migration, RLS — never translated)
- User-facing error messages NEVER expose technical state
  ("error", "system", "API") — frame as social courtesy
  ("Maaf, bisa tolong ulangi pertanyaannya?")
```

Without this split, Claude inherits one voice and applies it everywhere. UI copy ends up sounding like a Slack DM, or chat replies sound like a press release.

## §6. Bloat — important rules buried

**Symptom:** CLAUDE.md is 800 lines. New session, Claude follows rule on line 50 but misses rule on line 600. You repeat-add the same rule because you forgot it was already there.

**Fix:**

1. Move the **top 5 rules** (the ones that, if violated, cause production incidents) to a `STOP — READ THESE FIRST` block at the top.
2. Audit every 2 weeks: which rules have NOT been triggered in any session? Delete or archive them.
3. Split by domain if any single section is >150 lines: spin out `ARCHITECTURE.md`, `SECURITY.md`, etc., and link from CLAUDE.md.
4. Use tables instead of prose where structure allows — tables are scannable, prose isn't.

A CLAUDE.md that nobody (including Claude) reads end-to-end is worse than a shorter CLAUDE.md that Claude actually applies.

## Audit ritual (every 2 weeks)

- Open CLAUDE.md alongside the last 5 sessions of conversation log
- For each rule: did it ever get applied? If never → delete or archive
- For each repeated mistake in conversation: is there a rule covering it? If not → add one
- Check every `always` / `never` for contradictions

CLAUDE.md is a living spec. Stale rules silently rot.

## When to escalate

If you've applied all 6 fixes and Claude still misbehaves:

1. The issue is probably model selection or context, not the file. Try a different model.
2. Check whether your CLAUDE.md is actually being loaded — run `/memory` in Claude Code and verify it appears.
3. If Claude reads it but ignores it, you may have `@`-imports that aren't resolving. Inline the imported content as a test — if behavior changes, the import was the issue.
