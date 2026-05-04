# No domain jargon in user-facing text

> User-facing strings must use the AUDIENCE's vocabulary, not the BUILDER's. Even when the builder term is "the right word", if it requires explaining, it's jargon and must be replaced with the concrete user-experienced thing. Distinct from no-technical-leak (which hides system state) — this hides BUILDER vocabulary.

## The rule

User-facing strings must use vocabulary the AUDIENCE actually knows, not the BUILDER's domain vocabulary. If you'd need to explain the term in a parenthetical, replace the term itself.

**Scope: ALL communication, including chat with the founder.** This rule isn't restricted to end-user-facing product copy. It applies any time you write text someone reads — including:

- Strategic recommendations
- Technical option presentations
- Status reports
- Dispatch prompts to other sessions

If the founder has to ask "what does X mean", the rule was violated.

## Two distinct anti-patterns

### 1. Builder / dev jargon that the user shouldn't need to learn

- ❌ "Submit a PR" / "Open an issue" / "Fork the repo" → ✅ "Tulis di GitHub bentuk thread, gue bantu" / "Write a description on GitHub, I'll help"
- ❌ "Adopt a pattern" → ✅ "Pakai aturan / template ini" / "Use these rules / templates"
- ❌ "Tour SKU" / "Operator slug" → ✅ "Trip code" / "Tour company name"
- ❌ "Slot asset reconciled" → ✅ "Booking status updated"
- ❌ "Persona claim signature" → ✅ "Access control"

### 2. Internal product taxonomy that leaks to user

- ❌ "Operator dashboard" (when user IS the operator) → ✅ "Dashboard" or "Control panel"
- ❌ "Cross-operator scope" → ✅ "Akses ke beberapa partner" / "Access to multiple partners"
- ❌ "Tier-1 routing" → ✅ "Pertanyaan umum" / "Common questions"
- ❌ "Tenant isolation" → ✅ "Data lo terpisah dari customer lain" / "Your data is separated from other customers"

## Why

Builder vocabulary feels precise BECAUSE you built the system. To users:

- "PR" requires a GitHub mental model — newcomers don't have it
- "Pattern" requires the pattern-language framework — only senior devs use it casually
- "Operator" / "Tenant" / "Slug" / "Asset" are internal product nouns, not consumer reality
- "Reconcile" / "Idempotent" / "Webhook" are infrastructure, not user concern

Result of jargon in user-facing text:

- New users bounce at first interaction (don't understand the menu)
- Existing users feel stupid
- Brand voice becomes "technical" instead of "warm friend"
- Conversion drops at every jargon touchpoint

## Real incident that captured this rule

A kit's onboarding directive shipped with options like "Adopt a pattern from the kit" and "Contribute a PR". The kit's own AUTHOR did not understand the menu options. If the author doesn't get it, the newcomer audience definitely won't.

Three atomic commits to remove jargon — even after the first fix, the second commit caught more (different word: "PR / issue" instead of "pattern"), and a third commit added a permanent constraint to the directive: avoid jargon in opener menus and initial walks unless the user uses the term first.

## How to apply

### Test before shipping any user-facing string

1. **Read it as a stranger.** Would someone who doesn't know your codebase / domain understand the action they're being asked to take?
2. **The "explain it" test.** If you'd need to explain the term in a parenthetical, replace the term itself.
3. **The "describe what they DO" test.** Instead of naming the artifact ("PR", "issue"), describe the action ("write a description", "share your version").
4. **The audience-substitution test.** Swap your audience for someone who has never heard of your product / framework. Would they understand without a follow-up question?

### Concrete process for AI-facing prompts (when user will TALK to AI)

- Maintain a banned vocab list specific to the product surface
- Pre-define concrete-action-noun substitutes
- Rule: only introduce a technical term IF the user uses it first OR explicitly asks how-to. Defer until user signals readiness.
- When you DO introduce the term: describe it concretely, don't assume.

### Audit checklist for each user-facing surface

Apply when adding ANY new user-facing surface (not just at launch):

- [ ] **Chatbots / AI agents:** does any system message use "session", "context", "tool use", "system prompt", "memory file"?
- [ ] **Customer-facing chat:** does AI response use product nouns ("operator", "destination") as user-friendly OR slip into "SKU", "slug", "operator profile"?
- [ ] **Partner / publisher dashboards:** does any partner-facing copy mention "engagement metric", "reach" without context, "publish queue"?
- [ ] **Customer notifications:** does any notification say "hold expired", "slot reconciled" instead of plain-language equivalents?
- [ ] **Public docs:** any docs using internal jargon without first defining for non-tech audience?
- [ ] **Payment-related emails:** any using "VA", "QRIS", payment-provider names without explaining the action ("transfer to bank", "scan QR")?

## Sibling rule

[`10-no-technical-leak-in-user-copy.md`](10-no-technical-leak-in-user-copy.md) covers system-state leak ("error", "API timeout"). This rule covers builder-vocabulary leak ("PR", "operator", "asset"). Both fall under "audience is not the builder."

## Constraints

- Internal / admin-only screens: jargon may be appropriate there. Audit decision = per-screen, not blanket.
- For multilocale apps: audit each locale separately. Translating builder jargon doesn't fix it — it just translates the same wrong choice.
- Decide audience PER STRING. Same app may have customer-facing strings AND partner-facing strings with different appropriate jargon levels.

## Related

- [`10-no-technical-leak-in-user-copy.md`](10-no-technical-leak-in-user-copy.md) — sibling rule covering system-state leaks
- [`skills/voice-register-check.md`](../skills/voice-register-check.md) — operationalized scan for both rules
- [`14-bahasa-first-voice.md`](14-bahasa-first-voice.md) — voice register modes (founder ↔ user)
