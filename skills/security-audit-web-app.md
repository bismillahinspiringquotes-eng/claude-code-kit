---
name: security-audit-web-app
description: Use when auditing a web app frontend/edge layer for common security mistakes — server vs client boundary leaks, environment variables exposed to the browser, missing security headers, unsafe redirects, dangerous HTML rendering, middleware that can be bypassed. Examples use Next.js + Vercel, but checks apply to any modern web framework (Remix, SvelteKit, Astro, Laravel, Rails).
---

# Security audit — web app layer

You're auditing a web app's frontend/edge for the 6 most common web security mistakes that lead to real incidents. Examples use Next.js + Vercel because that's the kit's recommended stack. The same concepts apply to any modern framework — only the function names change.

## The 6 checks

### 1. Environment variables leaked to the browser

**What goes wrong:** a secret meant for server use only ends up in the JavaScript bundle. Anyone viewing the page source or network tab can read it.

**What to check:**
- List every environment variable used in the codebase. Grep for `process.env.` (Node.js / Next.js) or your framework's equivalent.
- For Next.js: any variable that starts with `NEXT_PUBLIC_` is shipped to the browser. Verify nothing sensitive uses that prefix (no API keys, no service-role keys, no signing secrets).
- For other frameworks: check the framework's rule for which variables get bundled (Vite uses `VITE_`, etc).
- Check `.env.example` is committed but `.env*` (real values) is in `.gitignore`.

### 2. Server-only code accidentally included in client bundle

**What goes wrong:** code with database access, secrets, or sensitive logic gets imported by a client component → bundled into browser.

**What to check:**
- Files using server-only secrets: which files import them? Trace the import chain.
- Any client-side file (marked `'use client'`, or in `pages/` non-`api/`, or in `components/` without server marker) that imports a server-only module = leak.
- For Next.js: server actions (`'use server'`) called from clients are OK. Server modules imported by clients are NOT OK.

### 3. Security headers missing or misconfigured

**What goes wrong:** missing Content-Security-Policy, X-Frame-Options, X-Content-Type-Options → XSS attacks succeed, clickjacking works, MIME-sniffing happens.

**What to check:**
- For Next.js: read `next.config.mjs`/`next.config.js`. Look for `headers()` function returning security headers.
- For Vercel: check `vercel.json` headers config.
- For other frameworks: equivalent middleware/config file.
- Required headers: `Content-Security-Policy` (no `unsafe-eval` in production), `X-Frame-Options: DENY` or `SAMEORIGIN`, `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Strict-Transport-Security` for HTTPS.
- Bonus: `poweredByHeader: false` for Next.js (don't advertise framework version).

### 4. Unsafe HTML rendering

**What goes wrong:** user input rendered as HTML → script injection runs in other users' browsers.

**What to check:**
- Grep for `dangerouslySetInnerHTML` (React) or `v-html` (Vue) or equivalent.
- For each match: where does the content come from? If user input or external API, it must be sanitized first (e.g. via DOMPurify) — note in audit if not.
- Check for any use of `eval()`, `new Function()`, `setTimeout(string)` — these run arbitrary strings as code.

### 5. Unsafe redirect / open redirect

**What goes wrong:** user-supplied URL used in a redirect → attacker redirects users to phishing site that looks like yours.

**What to check:**
- Grep for `redirect(`, `Response.redirect`, or framework equivalent — any case where the destination URL comes from user input (query param, form field, request body).
- For each: is the URL validated? Allowed-list of internal paths only, OR explicit check that protocol is `https://` and host matches your domain.
- `javascript:`, `data:`, `vbscript:` URLs in `href` attributes — should be blocked.

### 6. Middleware bypass / route-level guard miss

**What goes wrong:** middleware checks auth on most routes — but one route slips past because of pattern mismatch, or middleware is skipped for static files, or a new route is added without updating middleware.

**What to check:**
- Read the middleware config (`middleware.ts` for Next.js, equivalent for other frameworks). What does it match? What does it exclude?
- List every protected route. For each, verify either: (a) middleware covers it, OR (b) the route file itself has a server-side auth check.
- Common pitfalls: middleware excludes `/_next/`, `/api/`, `/static/` — ensure no protected routes accidentally land in those.
- Recommend: don't rely on middleware alone for auth — always have a server-side check inside the route handler too. Defense in depth.

## How to deliver the report

```
# Web app security audit — [project name]

## Critical (fix before next deploy)
1. [issue] — Evidence: [file:line]. Why critical: [reason]. Fix: [concrete step].

## Warnings (fix soon)
[same format]

## Verified clean
- [check name]
```

End with: "Critical: [N], warnings: [N]. Mau gue bantu fix nomor 1 dulu, atau lo review dulu?"

## Constraints

- Read actual files. Cite file path + line number.
- One concrete fix per issue.
- If a check doesn't apply (no middleware in the framework, etc) — say "N/A: [reason]" rather than fake passing it.
- Don't blanket-recommend tools (Helmet.js, etc) — recommend the specific config change for the actual codebase.
