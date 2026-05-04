---
name: voice-register-check
description: Use when auditing user-facing strings (UI copy, error messages, emails, notifications) for two failure modes — technical jargon leaking into user-facing text ("error", "API", "null", "system"), and dev/builder jargon being used where the audience is a non-engineer end-user ("operator", "PR", "slug", "asset"). Operationalizes the no-technical-leak and no-domain-jargon-user-facing rules.
---

# Voice register check

You're auditing user-facing strings for two distinct mistakes that both come from the same root cause: developer wrote text from their own perspective, not the user's.

## What you're checking

### Failure mode 1: Technical state leaking into user-facing copy

User shouldn't see "error", "system", "null", "undefined", "API failed", "rusak", "broken" — these are dev-side observations.

**Why this matters:** when a user sees "Error: Unable to fetch", they think the product is broken. When they see "Maaf, ada gangguan sebentar — coba lagi ya", they think it's a temporary hiccup. Same underlying state, totally different user perception.

**What to grep for in user-facing text:**
- Words: `error`, `failed`, `null`, `undefined`, `system`, `crashed`, `broken`, `rusak`, `gagal`, `tidak ada`, `not found`, `forbidden`, `unauthorized`
- Stack-trace-like artifacts: `0x`, function names visible to user, line numbers
- Server response codes shown to user: `400`, `404`, `500`

**The fix pattern:** rewrite from the perspective of a polite human messenger.
- ❌ "Error: User not found"
- ✅ "Akun ini belum kita kenal — coba cek email-nya?"
- ❌ "Failed to load data"
- ✅ "Belum bisa ambil datanya barusan, coba refresh ya"

### Failure mode 2: Builder/dev jargon used where user is non-engineer

If the user is a customer (not a developer), they don't know what these words mean:
- Builder jargon: `PR`, `pull request`, `commit`, `merge`, `branch`, `fork`, `pattern`, `slug`, `asset`, `deploy`
- Domain-internal jargon: `operator`, `tenant`, `workspace owner`, `superhost`, `endpoint`, `webhook`
- DB jargon: `record`, `entry`, `row`, `null value`, `field`

**Why this matters:** even if the user is technical, you're writing to their CUSTOMER persona, not their dev persona. Don't assume they speak your shop talk in this context.

**What to grep for:**
- The words above, in any UI string, email template, push notification, error toast, etc.
- File patterns: any string in `components/`, `pages/`, `app/`, email templates, copy tables in DB, locale files

**The fix pattern:** describe what the user does or sees, in their language.
- ❌ "You don't have any active operators"
- ✅ "Belum ada usaha yang lo daftarin di sini" (or in English: "You haven't added any businesses yet")
- ❌ "This asset can be cloned"
- ✅ "Bisa lo duplikat/copy"

## How to run the audit

1. **Identify user-facing string sources.** Common locations:
   - Component files in `components/`, `app/`, `pages/`
   - Locale/translation files: `locales/`, `messages/`, `i18n/`
   - Email template files
   - Database table for editable copy (e.g. `site_copy` in Supabase)
   - Toast / notification calls

2. **Grep for each red-flag word in those locations.**
   - For technical leak: `error`, `failed`, `null`, `undefined`, `system`, `rusak`, `gagal`
   - For builder jargon: `operator`, `tenant`, `endpoint`, `workspace`, `slug`, `asset`
   - Skip matches in: comments, console.log, internal-only screens (admin dashboards behind staff-only auth)

3. **For each match, decide:**
   - Is this string actually shown to end-users? (vs a developer-only screen)
   - If yes: is the word appropriate, or is it a leak?
   - Suggest replacement copy.

4. **Special check: error message templating.**
   - Many apps have a generic "error.message" passthrough. Find where backend errors get displayed to users.
   - Recommend: never pass raw `error.message` to user. Use a translation map: known error → friendly copy. Unknown error → generic friendly fallback.

## How to deliver the report

```
# Voice register audit — [project name]

## Summary
- Files scanned: N
- Technical-state leaks found: N
- Builder-jargon usages found: N

## Technical-state leaks

| File:line | Current copy | Suggested replacement | Severity |
|---|---|---|---|
| ... | "Error: User not found" | "Akun ini belum kita kenal..." | high |

## Builder/dev jargon usages

| File:line | Current copy | Suggested replacement | Notes |
|---|---|---|---|
| ... | "operator dashboard" | "dashboard usaha lo" | "operator" is internal term |

## Special concerns
- [Any pattern detected, e.g. "every error toast does `toast(error.message)` directly — recommend wrapping in copy mapper"]
```

End with: "Total [N] strings flagged. Mau gue bantu apply fixes batch (highest-severity dulu), atau lo review report-nya dulu?"

## Constraints

- For internal/admin screens (dashboard for staff only): skip — jargon is appropriate there.
- Don't auto-replace. Always show before/after for user to approve. Voice nuances matter.
- For multilocale: audit each locale separately. Don't translate from one locale to another and assume voice is preserved.
- Decide audience PER STRING — same app may have customer-facing strings AND partner-facing strings with different appropriate jargon levels.
- If unsure about a word: ask the user about audience for that screen. Don't guess.
