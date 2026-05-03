# CLAUDE.md — Aturan Ketat (yang bener-bener dipake)

> **For English readers (2-paragraph TL;DR).** This document is the public version of the CLAUDE.md ruleset run across 8 active production repositories. It exists because every "AI millionaire" tutorial skips the boring discipline part — the rules that prevent Claude from confidently shipping broken code. The body is in Indonesian (the real working voice); these rules don't translate cleanly without losing the register that makes them stick.
>
> If you're new: scroll to **rule #8 (SHIP VERIFICATION)**. That one came from a real production incident — a published article was orphaned from the production database for 24 hours because we trusted "commit pushed" instead of `curl`-ing the live URL. Every rule here exists because something specific broke. Steal what fits, fork the rest.

---

Pertanyaan paling sering dari developer Indo yang baru pegang Claude Code: **"rules ketat di CLAUDE.md seperti apa, bro?"**

Yang di bawah ini bukan teori. Bener-bener dipake di 8 repo production, hasil iterasi dari kejadian-kejadian yang bikin sakit kepala. Copy-paste, adaptasi, fork — bebas. Setiap rule ada **alasannya** (insiden nyata atau pain yang berulang), bukan ritual cargo-cult.

## 1. Token discipline (paling sering dilanggar)

```
- Cut 60% output fat. Meaning intact.
- No preamble. No filler. No "Great question!". No repeat.
- Report = table/bullets. Prose only when asked.
- Don't explain before executing — just do it.
- Max 1 confirmation per task. Not per step.
- Questions: max 1 at a time.
```

**Kenapa penting:** Claude tanpa rule ini bakal ngabisin 40% context window cuma buat sopan-santun, restate plan, dan "Let me think about this..." yang ga produktif. Token = uang. Token = context yang harusnya dipake buat reasoning, bukan filler.

**Pengecualian** (JANGAN dipotong):
- Code writing: full, proper, clean. Hemat token jangan di code.
- Deep read: baca tiap baris file. Hemat di output, jangan di proses.

## 2. Trigger word system

Kata-kata pendek yang Claude harus recognize sebagai command, bukan obrolan biasa:

| Trigger | Action |
|---|---|
| `start` | Read CLAUDE.md → README → SESSION.md → PROGRESS.md, lapor status singkat |
| `start [topic]` | Plus deep-load file topic-specific (`start payment` → load `docs/PAYMENT_PLAN.md` juga) |
| `accio!` | End-of-session ritual: update PROGRESS, SESSION, version bump, commit |
| `monyet!` | Mid-session: simpan strategic decisions ke `DECISIONS.md` |
| `catat itu` | Save apa pun yang barusan dibahas ke memory file paling relevan |
| `sync!` | Cross-repo pull: rebuild memory dari brief files semua venture |

**Kenapa pake bahasa Indo / kata aneh kayak `monyet!`:** biar ga collision sama natural conversation. "Save this" terlalu ambigu — bisa jadi Claude ngira lo lagi bilang ke orang lain di chat. `monyet!` jelas-jelas intentional. Friction = signal.

## 3. Never skim read

```
- ALWAYS read existing files before rewriting anything
- Never assume file structure
- NEVER skim read — read every line
- File > 200 lines: read in chunks, no skipping sections
- When auditing docs: check every fact, every version number, every status.
  No "looks OK" without verification.
```

**Kenapa:** Skim → assume → wrong file structure → write code yang break existing functionality. Pernah Claude rewrite component karena "looked similar" tanpa baca tiap baris — ternyata ada conditional rendering yang ke-overwrite, regression baru ketauan 2 hari kemudian. Fix-nya 3x lebih lama daripada baca file dari awal.

Hemat token di **output**, jangan di **proses**.

## 4. Confirmation sebelum code/push (tapi di-balance)

```
- Before coding → "Gue tangkep: [X]. Bener?"
- Ambiguous instruction → "Lo maksud A atau B?"
- Contradicts previous decision → "Heads up — lo udah decide [X]. Override?"
```

Yang sering ke-miss: rule sebaliknya juga harus ada, biar Claude ga jadi over-cautious.

```
- When user says "execute" / "go" / "ya" → execute, don't restate plan.
- Trust granted = ship it.
```

Tanpa pasangan kedua ini, Claude bakal nanya "Mau gue lanjut?" 3x dalam satu task. Boros.

## 5. Mid-session checkpoint (70% rule)

```
- Context approaching 70% → proactive checkpoint
- Say: "Context 70%. Checkpoint saved. Safe to /clear."
- Never hit 100% without saving
- Next session reads checkpoint → continues from exact spot
```

**Kenapa:** Context window 100% = forced auto-summarize yang bisa drop info penting (decision rationale, file paths yang udah di-recon, edge case yang udah di-flag). 70% = masih ada ruang buat write coherent checkpoint. Proactive, bukan reactive.

## 6. Voice register modes

```
- With founder (chat session): Indonesian casual — gue/lo
- With end user (UI copy, error message, email): universal aku/kamu
- With end user, customer service tone: NEVER expose technical state
  ("error", "rusak", "system error" → frame as social courtesy:
   "Maaf, bisa tolong ulangi pertanyaannya?")
- Tech identifiers: tetap English (API route, migration, RLS — jangan
  diterjemahin "rute API")
```

Ini paling sering di-violate sama default Claude. Bawaan-nya formal Indonesian (saya/anda) yang mati di chat dan terlalu corporate di UI. Lock register-nya di CLAUDE.md, jangan harap Claude nebak sendiri.

## 7. Cross-repo rule import (kalau lo punya banyak repo)

Kalau lo running multiple repos dan rule yang sama (voice, deploy discipline, security baseline), simpan di canonical location, import via `@`:

```markdown
## Cross-Holding Memory (auto-loaded at session start)

@~/path/to/canonical/memory/INDEX.md

If `@`-import not resolved by your Claude Code version:
explicitly read the file at session start.
```

**Kenapa:** Single source of truth. Update sekali → semua repo dapat. Anti-drift. Tanpa ini, lo bakal punya 8 versi rule "deploy verification" yang slowly diverge.

**Anti-pattern:** jangan symlink atau copy file rule ke tiap repo — itu bikin drift. Import-by-reference only.

## 8. ⭐ SHIP VERIFICATION RULE (the one that matters most)

Yang ini lahir dari production incident yang paling mahal sejauh ini:

> **"Shipped" ≠ MD ditulis. ≠ Commit pushed. ≠ Deploy ran. ≠ TypeScript pass.**
>
> **"Shipped" = user-facing URL returns the actual content.**

**War story (sanitized):** sebuah article yang udah di-write + di-commit + di-push dianggap "shipped" karena tiap step kelihatan success. Ternyata 24 jam orphaned dari production database — page rendering baca dari DB, bukan filesystem. User akses → 404. Yang di-deploy adalah artifact, bukan content.

Fix-nya: verify dengan `curl` ke production URL setelah deploy. Cek **body content match expectation**. Bukan "200 OK" doang — content match.

**3-gate rule yang lahir dari sini:**

```
Gate 1: tsc --noEmit must pass        ← type check
Gate 2: npm/pnpm build must pass      ← actual build (lint-as-error included)
Gate 3: curl prod URL → content match ← user-facing reality
```

Type check passing ≠ build success. Build success ≠ user-facing works. Tiap gate independent. Skip salah satu = roulette.

Pernah ada juga incident TypeScript pass tapi Vercel build silently failed karena lint-as-error di production mode. 24 jam serve stale code. Lesson: gate 2 is not optional.

---

## Cara pake rules ini

1. **Fork repo `claude-code-kit`**, copy `templates/CLAUDE.md.template` ke repo lo
2. **Adaptasi**: ganti placeholder dengan stack + business context lo (jangan dipake apa adanya — half the value is the customization)
3. **Iterate**: tiap insiden yang bikin sakit kepala → tambah rule + alasannya
4. **Audit tiap 2 minggu**: rule mana yang ga pernah ke-trigger? Bisa dihapus. CLAUDE.md yang bloated = ga ada yang baca.

## Penutup

These rules evolve. PRs welcome. Open an issue with patterns from your own setup — kalau pattern lo lebih elegant, gue ganti. Goal-nya bukan ngumpulin rule terbanyak, tapi ngumpulin rule yang **paling sering nge-save dari production incident**.
