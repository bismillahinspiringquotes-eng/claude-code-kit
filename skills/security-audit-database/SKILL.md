---
name: security-audit-database
description: Use when auditing a database-backed app for the most common database security mistakes — multi-tenant data leaks, admin keys exposed to the browser, missing auth guards, unencrypted personal data, public endpoints returning sensitive fields, webhook handlers without signature checks. Examples below use Supabase + Next.js, but the same checks apply to any database with row-level security (Postgres, Firebase, MongoDB Atlas, etc.).
---

# Security audit — database layer

You are auditing a database-backed web app for the 7 highest-stakes database security mistakes. Each one has caused real production incidents. Read actual files for evidence — don't assume.

Examples below use Supabase + Next.js because that's the kit's recommended stack. For other databases (Postgres direct, Firebase, MongoDB), the same concepts apply — just the function names change.

## The 7 checks

### 1. Multi-tenant data leak (highest stakes)

**What goes wrong:** one customer queries data → result includes another customer's rows. The most expensive incident class — usually trust-destroying once it happens.

**What to check:**
- Every table that holds customer/tenant data should have row-level security enabled. In Supabase migrations: grep for `ENABLE ROW LEVEL SECURITY`. Any table with customer data missing this = critical.
- Every query that reads tenant data should filter by tenant ID server-side, not trust client input. Grep for `from('table_name')` calls — for each, check if the tenant filter is in code or relies on RLS alone.
- Any use of admin/service-role client that bypasses RLS needs a comment explaining why. List every occurrence with no comment.

### 2. Admin/service-role key exposed to the browser

**What goes wrong:** the key that lets server skip all security checks ends up in the JavaScript bundle the browser downloads. Anyone can read it. Game over.

**What to check:**
- Grep for service-role variable names (e.g. `SUPABASE_SERVICE_ROLE_KEY`, `*_SECRET_KEY`) across the entire codebase.
- For Next.js: any match in a file marked `'use client'`, or imported by one, is a leak.
- For any framework: any match in a file inside `app/`, `pages/`, `components/` (without `'use server'`) is a leak.
- Verify env var prefixes: anything starting with `NEXT_PUBLIC_` is shipped to browser. Service keys must NOT use that prefix.

### 3. Auth check NOT the first line of every API route

**What goes wrong:** logic runs before auth check → unauthenticated callers can trigger side effects, leak data, or rate-limit-bypass.

**What to check:**
- For each file in `app/api/` (Next.js) or equivalent server route folder: read the first 5 lines of the handler. Auth check must be there.
- Acceptable: routes explicitly listed as public (signup, login, public webhooks with signature check).
- Not acceptable: "we'll check auth later in the function" — function early-returns or throws can be bypassed.

### 4. Personal data stored unencrypted

**What goes wrong:** database backup leaks → email/phone/passport readable. Regulatory risk + customer trust damage.

**What to check:**
- List columns containing personal data: email, phone, passport, ID number, full name, address, bank account, payment details.
- For each: is it encrypted at the application layer before write? Grep for the column name in write paths. Encryption helper should be called before insert/update.
- Bonus: check if the column is in any public-facing query response. If yes — should not return it, encrypted or not.

### 5. Public endpoint returns sensitive fields

**What goes wrong:** developer thinks "internal field, not visible" — but it's in the JSON response, just not rendered. Anyone reading the network tab sees it.

**What to check:**
- For each public endpoint (no auth required), read the response body. List every field returned.
- Cross-check: does any field contain customer identifier (email, phone), internal-only state (`hold_source`, `created_by`, `internal_notes`), or pricing/cost fields not meant for the customer?
- Pay attention to `select('*')` — that returns every column, including any added later. Recommend explicit column lists.

### 6. Webhook handler skips signature verification

**What goes wrong:** anyone can POST to the webhook URL with a fake payload. Marks payments as paid that weren't, triggers refunds, etc.

**What to check:**
- For each webhook endpoint (payment provider, partner integrations): is there a signature/token check BEFORE any state mutation?
- Comparison must use a constant-time function (e.g. `crypto.timingSafeEqual`) — `===` leaks information via timing.
- The secret used for verification should be a dedicated webhook secret, NOT the database service-role key reused.

### 7. Migration drops a policy without checking what depends on it

**What goes wrong:** dropping a row-level security policy in a migration → all queries that relied on it now over-permissive. Often invisible until something leaks.

**What to check:**
- Read recent migration files. Any `DROP POLICY` or `ALTER POLICY` statements?
- For each: grep the codebase for the table name. If non-admin clients query that table, the migration risks regression.
- Recommend: every policy drop should be paired with verification that no client code path relies on it.

## How to deliver the report

Single report, three sections:

```
# Database security audit — [project name]

## Critical (fix before next deploy)
1. [issue] — Evidence: [file:line]. Why critical: [explanation]. Fix: [concrete step].

## Warnings (fix soon)
[same format]

## Verified clean
- [check name]
- [check name]
```

End the report with one of:
- "Critical issues: [N]. Mau gue bantu fix yang nomor 1 dulu, atau lo review report-nya dulu?"
- "Critical: 0, warnings: [N]. Mau gue bantu fix warnings, atau biarin?"

## Constraints

- Read actual files. Don't audit by intuition.
- Cite file path + line number for every finding.
- One concrete fix per issue. Don't list 5 "consider" items per finding.
- If you can't determine status (e.g. file missing, ambiguous), say so explicitly — don't fake confidence.
