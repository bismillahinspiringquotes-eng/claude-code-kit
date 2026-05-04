# Destructive action discipline

> Before suggesting any command that can mutate user state — file writes, env edits, git operations, DB queries, deploys — VERIFY the command's actual behavior, EXHAUST recovery vectors first if extracting data, and USE your own tools to verify, don't delegate verification to the user. Captured from the most-expensive incident class: the founder's local environment file destroyed by a CLI command that "looked right" but wasn't.

## The rule

Before suggesting ANY command that can mutate user state:

1. **VERIFY the command's actual behavior before suggesting it.** Don't guess from documentation. Don't assume CLI behavior matches mental model. Test in safe context first OR explicitly flag uncertainty.

2. **EXHAUST RECOVERY VECTORS BEFORE EXTRACT.** If the goal is to extract a secret/value/data the user already has somewhere, search non-destructively first:
   - Past Claude session transcripts (often contain pasted values)
   - Shell history (`~/.zsh_history`, `~/.bash_history`)
   - Editor local history (e.g. VS Code `~/Library/Application Support/Code/User/History/`)
   - Filesystem snapshots (APFS local snapshots, Time Machine)
   - Project's git history including reflog
   - Sibling files (`.env.local.example`, `.env.bajo-temp`, etc.)
   - Other projects where the same value might be reused

   ALL of these BEFORE suggesting destructive extract.

3. **USE YOUR OWN TOOLS TO VERIFY.** When you have Bash, Read, Write tools, use them. "Run this and paste output" is wrong when you can run it yourself. The user shouldn't be your terminal.

## Why

This rule exists because of an incident that destroyed the founder's local environment file with a CLI command suggested without verification.

**The chain of failure:**

1. User asked how to extract a secret marked "Sensitive" from a deploy platform.
2. AI suggested `[platform-cli] env pull --cwd ~/path .env.temp` without verifying the CLI's file-write behavior.
3. The CLI's `--cwd` flag changes the project context BUT writes the output file to the project's directory, not the current working directory. AI didn't know this. Didn't test. Guessed.
4. After several broken attempts, the file landed where AI didn't expect. AI then asked overwrite of `.env.local` (the user's REAL local environment file with personal overrides).
5. User's `.env.local` got overwritten with production values — where sensitive vars were ALSO empty because the platform's "sensitive" flag blocks CLI export too (AI didn't know this either).
6. Original `.env.local` content = permanently gone.
7. Worse: prior session transcripts already contained the actual token values from earlier sessions where the user had pasted them. Recovery was sitting in transcripts the whole time. Should have searched FIRST.
8. Throughout the recovery, AI told the user "verify this", "run this and paste" when AI had Bash tool and could have run everything itself.

**What it cost:**
- Founder's local environment with personal overrides = lost forever
- ~30 minutes of recovery work, multiple back-and-forth turns
- Trust damage (highest-stakes consequence in the entire conversation history that this rule was extracted from)

**Root anti-pattern:** suggesting destructive operations based on plausible-looking knowledge without verification. The CLI command "sounded right", "looked right", was wrong in a destructive way.

## How to apply

### Pre-suggestion checklist for any state-mutating command

Before recommending it, all should be ✅:

- [ ] Have I run this exact command in a safe context (Bash tool, `/tmp` dir) and seen the output?
- [ ] Have I read `--help` to verify flag behavior — especially `--cwd`, `--output`, `--force`?
- [ ] Could the command's default behavior overwrite something? Have I flagged that risk?
- [ ] Is there a non-destructive alternative I should try first (`--dry-run`, read-only mode)?
- [ ] If the goal is extract: have I searched local recovery vectors first?
- [ ] Could I run this myself with Bash tool instead of asking user to run?

### For sensitive value extraction specifically

```
1. Search past Claude session transcripts for the value/key name
2. Search shell history
3. Check sibling .env.* files in the same project
4. Check git reflog for any accidental commits
5. ONLY THEN consider CLI extract
6. If CLI extract: VERIFY it doesn't overwrite anything, run in /tmp
```

### For file operations specifically

- `cd /tmp && [destructive command]` is NOT safety if the command writes to a path outside `/tmp`. Verify destination.
- Always test with `--dry-run` flag if it exists.
- For platform-specific env-pull commands: verify output destination BEFORE running.
- For `mv`, `rm`, `cp`: backup first, OR use `git stash`.

### For verification specifically

- If you can run `ls -la X` yourself, don't ask user to run it.
- If you can `cat`, `grep`, `diff` files yourself, don't ask user.
- If you need user to confirm a sensitive value's identity, ask ONCE with multiple candidates surfaced — don't ping-pong rounds of "maybe this one? no, maybe that one?"

## Specific gotchas to internalize

For future sessions, hard-coded knowledge from the original incident:

1. **"Sensitive" env vars on most deploy platforms are blocked from CLI export**, not just from dashboard view. CLI export returns sensitive values as empty strings. Recovery requires (a) the user's external backup (1Password, etc.), (b) search past session transcripts where values may have been pasted, OR (c) rotate the value via the underlying service's admin console.

2. **`--cwd PROJECT` on many CLIs writes the output file to PROJECT dir**, not the current pwd. The positional arg as filename can be inconsistent across CLI versions. Test before recommending.

3. **Many env-pull CLIs overwrite without warning by default.** A `--yes` or `--force` flag exists to auto-confirm overwrite prompts — but the default is usually "overwrite, no warning". Run in `/tmp` ONLY if you've actually `cd /tmp` AND the destination respects pwd.

4. **Always search past session transcripts for sensitive values BEFORE suggesting any extract.** Users paste values into AI sessions all the time. Transcripts persist. This is faster + safer than any CLI flow.

## Constraints

- This rule overrides default-to-execute when ops are destructive. Default-to-execute = "use your tools instead of asking user". Destructive-action-discipline = "verify your tools' behavior before using them, especially the ones with `--force` defaults".
- Don't blanket-warn on every Bash command. The discipline applies specifically to commands with mutating side effects on user files / env / state. `ls`, `grep`, `find` don't need this gate.
- For commands that touch deploy state (pushing branches, redeploys, DNS changes): same discipline applies — verify behavior before triggering.

## Related

- [`06-audit-existing-before-build.md`](06-audit-existing-before-build.md) — same family: verify before claim/build, just extended scope
- [`08-research-paper-full-read.md`](08-research-paper-full-read.md) — same family: verify source before claim, applied to research instead of commands
- [`05-default-to-execute.md`](05-default-to-execute.md) — default-to-execute YIELDS to this rule when ops are destructive. Not a contradiction; a hierarchy.
