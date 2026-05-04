---
name: secret-rotation-walkthrough
description: Use when a secret has been exposed (committed to git, leaked in logs, shared accidentally) and needs immediate rotation across all systems that use it, OR when running a scheduled rotation. Walks through the full rotation cycle without missing a system. Examples cover Supabase keys, Vercel env vars, payment provider secrets, webhook tokens — but the protocol applies to any secret.
---

# Secret rotation walkthrough

You're guiding the user through rotating a secret. This skill exists because rotation is one of those tasks where missing one step (one system that still uses the old secret) means the rotation didn't actually fix the leak — or worse, broke production.

## When to use this

Two cases:
1. **Emergency rotation:** secret was exposed (committed to git, posted in chat, screenshot leaked, employee left). Old secret must be invalidated NOW.
2. **Scheduled rotation:** quarterly hygiene, no known leak. Less urgent but same protocol.

For emergency: there's a "minimize damage" phase BEFORE rotation. For scheduled: skip ahead.

## Phase 0 (emergency only): contain the damage

Before generating new secret:
1. **Confirm the leak scope.** Where exactly was it exposed? (committed to which repo, posted in which channel, screenshotted by whom, etc.) The scope determines who could have seen it.
2. **Check for active misuse.** Look at recent activity logs of the service the secret accesses. Any requests from unexpected IPs / unexpected times / unusual patterns since the leak?
3. **If active misuse:** consider revoking the secret immediately (read-only mode for the service if available) BEFORE generating replacement. Better short outage than ongoing damage.

## Phase 1: identify every system that uses this secret

Before generating the new value, list everywhere the OLD value exists. Otherwise rotation will break things.

For Supabase service-role key, common locations:
- Local `.env.local` on developer machines
- Vercel environment variables (production, preview, development scopes)
- VPS environment files (e.g. `/etc/something/.env` for cron worker)
- GitHub Actions secrets (if CI uses it)
- Any other deploy platform (Cloudflare Workers, Netlify, etc)
- Backup/snapshot scripts running on a server
- Internal tooling (admin scripts a teammate runs locally)

For payment provider secret (Xendit, Stripe, etc):
- Same list as above
- Plus: the webhook configuration on the provider dashboard (signature secret)
- Plus: any third-party integration that calls the API on your behalf

For Cloudflare API token:
- Wherever DNS automation runs
- Wherever cache purges run
- Wherever certificate provisioning runs

**Deliverable for this phase:** a numbered list of every system using the secret. Don't proceed until the list feels complete. Ask user if you're missing any.

## Phase 2: generate the new secret

- Generate via the provider's official UI or CLI. Never reuse old values, never derive from old values.
- For Supabase: project settings → API → reset service role key.
- For Xendit / Stripe: dashboard → API keys → create new + record old key for revocation later.
- For Cloudflare: tokens UI → create new with same scopes.

Save the new value somewhere temporary but secure (your password manager). Don't paste into chat (this counts as another leak).

## Phase 3: deploy new secret to every system, in safe order

Order matters. If you update the API server first but not the workers, workers will fail with old secret. If you revoke before deploying everywhere, everything fails.

**Safe order:**
1. **Update systems that READ data first** (lower stakes if they're briefly down).
2. **Update systems that WRITE data second** (higher stakes — you want them to keep working through the change).
3. **Update webhook receivers LAST** — they're often the most fragile.

For each system:
- Update env var with new value.
- Restart / redeploy the service so it picks up the new value.
- Verify the service still works (curl an endpoint, check logs).
- THEN move to next system.

For Vercel specifically:
- Update env var in Vercel dashboard (set in all environments: production, preview, development).
- Trigger a deploy (push a no-op commit, or use Vercel "Redeploy" button).
- Verify the deployed app still works.

For VPS-based services:
- Update `.env` file on the VPS.
- Restart the service (`pm2 restart worker-name`, `systemctl restart service`, etc).
- Tail logs for 1-2 minutes to confirm clean restart.

## Phase 4: revoke the old secret

Only after Phase 3 is done across ALL systems.

- For most providers: dashboard has a "revoke" or "delete" action on the old key.
- For some providers (Cloudflare, AWS): old keys remain valid until explicitly deleted — delete them.
- For self-managed (HMAC secrets, custom tokens): the new value replacing the old in env effectively revokes if there's no fallback.

**Verify revocation:** try to use the OLD value to make a request. Should fail (401, 403). If it still works, revocation didn't happen — investigate.

## Phase 5: incident postmortem (emergency only)

If this was an emergency rotation:
1. Write a brief postmortem: how did the leak happen? Was it a process gap (no `.gitignore`?), a behavior gap (committed real key into example file?), or a system gap (CI logs printed env vars)?
2. Add a rule to the project's CLAUDE.md to prevent recurrence. One-liner with **Why:** referencing this incident.
3. If multiple secrets in the same repo could leak the same way, consider a one-off audit: grep for likely-secret patterns (`sk-`, `eyJ`, long random strings) across full git history.

## How to deliver the report

After each phase:
```
✅ Phase [N] complete: [summary]
[any caveats]
Next: Phase [N+1].
```

Final report:
```
✅ Rotation complete for [secret name].

Rotated across [N] systems:
- [system 1]: ✅ verified
- [system 2]: ✅ verified
- ...

Old secret revoked: ✅ confirmed (test request returned [error code]).

Postmortem rule added to CLAUDE.md: [yes/no]
```

End with: "Selesai. Mau gue bantu nge-grep history repo buat secret-pattern lain (jaga-jaga), atau lo udah comfortable?"

## Constraints

- Never paste actual secret values into chat or logs. Even partially.
- Never skip Phase 1 (identify all systems). Most rotation failures come from missing one system.
- Never skip Phase 4 verification (test that old secret is actually invalid). "I clicked revoke" ≠ "the key is dead".
- For emergency rotation: timer matters. Track elapsed time between leak detection and revocation. Should be minutes, not hours.
- If the user says "I'll do the actual rotation later" — push back: leaked secrets get exploited fast. Better partial rotation now than scheduled rotation never.
