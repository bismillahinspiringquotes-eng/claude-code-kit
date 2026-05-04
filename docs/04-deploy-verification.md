# Deploy verification: the 3-gate rule

> Type-check passing ≠ build succeeded ≠ user-facing surface works. Three gates, in order, every time. Skip a gate = roulette.

## The rule

Every "shipped" claim must pass three gates. In order. No skipping.

```
Gate 1: tsc --noEmit  (or equivalent type/lint check)
Gate 2: production build succeeds  (e.g. pnpm build, next build)
Gate 3: curl the live URL → response body matches expected content
```

"Shipped" = Gate 3 passes. Not "MD written", not "commit pushed", not "deploy ran". The user-facing URL serves the content you intended.

This is a methodology doc. The operationalized check is in [`skills/pre-deploy-3gate.md`](../skills/pre-deploy-3gate.md).

## Why three gates (not one or two)

Each gate catches failure modes the others miss.

### Gate 1 alone is dangerous

Type-check uses dev-mode tooling. Production build uses different rules — lint-as-error, tree-shaking, output verification. A common silent failure mode: type check passes locally, production build fails on lint-as-error in CI/cloud platform, deploy is recorded as "failed" but stale code keeps serving from previous successful deploy. Looks fine to an unmonitored eye.

**Real incident:** type check passed, production build silently failed on an unused variable that the dev-mode lint tolerated. Production served stale code for 24 hours before the gap was caught.

### Gate 2 alone is dangerous

Build success means YOUR CODE compiles. It does NOT mean YOUR USERS see the change. The change might be:

- Cached at the CDN layer (old version still served)
- Rendered from a database that wasn't updated (filesystem code is fresh, DB rows stale)
- Behind a feature flag that's still off
- Routed through a worker that didn't restart
- Geo-blocked / region-stuck (deployed to one region, traffic hits another)

**Real incident:** an article was written as a Markdown file, committed, pushed, deploy succeeded. Article was orphaned from the production database for 24 hours because the rendering route read from DB, not filesystem. Page returned 404 the entire time. Discovered only when curl-from-the-outside was finally run.

### Gate 3 needs content match, not just status code

200 OK is not enough. The body content must match expectation. A deployed page can return 200 with empty content, with old-cache content, with placeholder content, with another tenant's content, with a 200 page-not-found template.

`curl https://yoursite.com/article-slug` → body must contain "[expected unique substring]".

For authenticated routes: probe with curl confirms the route EXISTS (returns 401/302 vs 404). Full render verification needs an authenticated session — delegate to a real browser hit + screenshot.

## How to apply

### In every project

Add the 3-gate ritual to your end-of-session protocol (see [`03-session-rituals.md`](03-session-rituals.md) — the `accio!` ritual should close with Gate 3).

Add to your CLAUDE.md `Pre-commit checklist`:
```
- Gate 1: type check passes
- Gate 2: production build succeeds
- Gate 3: curl deployed URL → body matches expected
```

### Wire deploy-failure alerting

Don't trust silent CI. Wire a failure alert into a channel you actually watch (Telegram, Slack, Discord). Many platforms have webhook integrations for deploy success/failure events.

Until alerting is wired, manual Gate 2 verification means CHECKING the deploy log status, not just running `git push` and assuming.

### After any change to lint/type/build config

Run Gate 2 locally before pushing. CI may have different env, different node version, different cache state — surface failures locally first, not in production.

### For database-driven content

If content is rendered from DB (articles, products, tours, etc.):
- Gate 3 must verify the data ALSO exists in production DB, not just that the route renders.
- A page that loads from DB can render successfully with empty / wrong / stale data.
- Specific check: `select * from <table> where slug = 'X'` against production DB.

## What this rule replaces

The single-gate "I deployed therefore it shipped" assumption. Three gates eliminate three independent silent-failure classes.

## Common pushback (and why it's wrong)

> "Just curl is overkill. Build passed."

Build passing is gate 2. The whole point of gate 3 is that gate 2 doesn't tell you what users see. The 30 seconds of curl-and-grep are far cheaper than a 24-hour silent-failure incident.

> "I'll add curl verification later."

The longer "later" is delayed, the more silent failures accumulate. Wire it now.

## Related

- [`skills/pre-deploy-3gate.md`](../skills/pre-deploy-3gate.md) — operationalized skill (run this before claiming shipped)
- [`01-claude-md-rules.md`](01-claude-md-rules.md) — SHIP VERIFICATION RULE (rule #8)
- [`02-fixing-broken-claude-md.md`](02-fixing-broken-claude-md.md) — fixing CLAUDE.md that doesn't enforce ship verification
