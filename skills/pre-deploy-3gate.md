---
name: pre-deploy-3gate
description: Use right before a production deploy or when you're about to claim work is "shipped". Runs the 3-gate verification (type check passes, build succeeds, live URL serves correct content) and refuses to call work shipped until all three gates pass. Operationalizes the SHIP VERIFICATION RULE from docs/01-claude-md-rules.md. Stack-agnostic; examples use Next.js + Vercel.
---

# Pre-deploy 3-gate verification

You are gate-keeping a production deploy. The user is asking you to verify their change is actually shipped — not just typed, not just committed, not just deployed. ACTUALLY working at the user-facing URL.

This skill exists because three real incidents shaped it:
- Type check passing while build failed silently → prod served stale code for 24 hours
- Commit pushed + deploy ran while published article was orphaned from production database for 24h (page rendered from DB, not filesystem)
- Webhook subscription disappeared between morning verification and afternoon production traffic

The 3 gates exist BECAUSE each of those was missed.

## The 3 gates

Run them IN ORDER. Don't skip ahead. If gate N fails, stop — don't run gate N+1 until N passes.

### Gate 1: Type check passes

**Why this gate exists:** if type check fails, you have a code error before even attempting build. No point continuing.

**How to run:**
- For Next.js / TypeScript: `npx tsc --noEmit` or `pnpm tsc --noEmit`.
- For other typed languages: equivalent type-check command.
- For untyped projects (plain JS): use the linter as the gate — `npm run lint` with strict config.

**Pass criteria:** exit code 0, no errors. Warnings can be acknowledged but not blocking.

**If failed:** report the specific error file + line. Don't proceed.

### Gate 2: Production build succeeds

**Why this gate exists:** type check uses dev-mode tooling. Production build uses different rules — lint-as-error in production, tree-shaking, output verification. A common silent failure: type check passes, prod build fails on lint, deploy platform records "build failed" but cron / scheduled deploys mask the failure.

**How to run:**
- For Next.js: `pnpm build` or `npm run build`.
- For Vercel: this is what runs server-side on every deploy.
- For other platforms: their build command.

**Pass criteria:** exit code 0, build artifact produced (`.next/` for Next.js, `dist/` for many tools, etc.).

**If failed:** report the specific error. Common: lint-as-error, missing env var at build time, import resolution failures. Don't proceed.

### Gate 3: User-facing URL serves correct content

**Why this gate exists:** type check + build success means YOUR CODE compiles. It does NOT mean YOUR USERS see the change. The change might be:
- Cached at CDN (old version still served)
- Rendered from database that wasn't updated (filesystem code is fresh, DB rows are stale)
- Behind a feature flag that's still off
- Routed through a worker that didn't restart

**How to run:**
- Identify the exact URL where the change should be visible (e.g. `https://yoursite.com/articles/the-new-one`).
- `curl -s -o response.html -w "%{http_code}\n" "https://yoursite.com/path"`.
- Read `response.html`. Verify the body contains the EXPECTED content (specific text, specific element, specific data).

**Pass criteria:** HTTP 200 AND response body contains the expected content. NOT just "200 OK" — content must match.

**If failed:**
- 404: routing not deployed. Check your platform deploy logs.
- 500: runtime error. Check logs.
- 200 but old content: CDN cache, or DB-driven content not updated, or feature flag off. Verify each possibility.

## How to deliver the report

After each gate, report:
```
Gate N: [pass/fail]
[command run]
[exit code or HTTP status]
[expected vs actual where relevant]
```

After all 3 gates pass:
```
✅ All 3 gates passed.
- Type check: clean
- Build: succeeded
- URL [https://...] returns expected content (snippet: "[matched substring]")

Safe to claim shipped.
```

If any gate fails:
```
❌ Gate [N] failed. Stopping.

[failure detail]

NOT shipped until this is fixed.
```

End with: "Mau gue bantu fix gate [N], atau lo handle dulu?"

## Constraints

- NEVER claim shipped just because two of three gates passed. All three.
- NEVER skip gate 3. "Build succeeded so it's deployed" is the exact assumption that caused the original incidents.
- For multi-page changes: run gate 3 against AT LEAST 2-3 representative URLs, not just one.
- For database-backed content (articles loaded from DB, products listed from DB): verify the data also exists in production DB, not just that the route renders. A page that loads from DB can render successfully with empty/wrong data.
- If the user pushes back ("just curl is overkill, build passed") — refer them to the war stories above. The war stories are why this skill exists.
