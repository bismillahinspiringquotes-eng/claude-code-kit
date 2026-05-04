# Audit existing before build

> Before building any new component, feature, schema, or external dependency, AUDIT what already exists. Reuse + refine > build alongside duplicate. Captured the day this rule was violated three times in three separate sessions.

## The rule

Before building ANY new component, feature, schema, library, or infrastructure dependency:

1. **Grep the codebase** for the concept (component name, feature noun, similar functionality)
2. **Check existing planning docs** (`.planning/`, `ROADMAP.md`, `ARCHITECTURE.md`)
3. **Read existing similar files in full** — not skim
4. **Determine: extend existing OR genuinely new**
5. **If existing covers ~50%+ of the need** → extend / refine that component, don't build alongside

The principle, paraphrased: *what already exists shouldn't be tweaked — just reuse and refine with whatever's additionally needed.*

## Why

Building duplicates instead of extending:

- Creates two systems to maintain, forever
- Confuses users (which component does what?)
- Wastes the prior design work
- Violates DRY at the architecture level
- Introduces inconsistency between the two implementations

## Three real incidents that caught this rule

All three happened in a single day, across different projects:

**Incident 1 — duplicate UI component.** A "Topic Tiles" component was built alongside an existing "Expert Guides" accordion that already covered the same content categories per a Lonely Planet template. 3 of 5 categories duplicated. Should have transformed the accordion into cards in-place, not built a parallel component.

**Incident 2 — proposed external platform that already existed internally.** A self-hosted external messaging platform was proposed for a chat feature. After audit: the codebase already had 3,400+ lines of chat infrastructure (component dirs for chat + messages, customer / partner / admin dashboards, DB tables, server actions). The external platform would have duplicated all of it. Should have audited `components/chat/` and `components/messages/` BEFORE proposing the external option.

**Incident 3 — example SQL written with fabricated literals.** Example SQL for a token feature got written in conversation with made-up scope literals and wrong column semantics. The actual values were verifiable from `src/lib/api-types.ts` enum definitions + a planning spec. Should have grepped the enum + read the migration BEFORE writing example code.

The strawman-SQL trap: example code in conversation gets copy-pasted to verbatim production runs faster than expected. "It's just an example" framing is dangerous — recipient assumes correctness. Treat ALL code written in conversation as if it could be executed verbatim; verify schema + literals first.

Same root anti-pattern across all three: didn't audit existing before recommending / proposing / exemplifying new.

## How to apply

### For new components

```
find . -iname "*<concept>*" -type f
grep -ri "<concept>" components/ src/ --files-with-matches
```

Read the 2-3 most-relevant existing files BEFORE designing the new one.

If existing covers part of the need: design as extension, not parallel.

### For new external dependencies

(Chatwoot / Sendbird / Stream / Auth0 / etc.)

1. List the features the external platform provides
2. Audit each feature against existing codebase: do we already have it?
3. If 50%+ exists internally: thin layer on top of existing > deploying new platform
4. Check `agents/`, `.planning/` for shipped or scoped features

### For new schemas / tables

```
grep -i 'CREATE TABLE' supabase/migrations/*.sql | grep -i <concept>
```

Check if extending existing table (new columns) > creating new table (new joins, new RLS).

### For new pages / routes

```
find app -name "page.tsx" | xargs grep -l <concept>
```

Check if existing route can host new content vs creating sibling route.

### For example code in conversation

Before writing `INSERT INTO ...` / `SELECT ... FROM ...` / similar in chat:

- Grep the actual enum / column names from the codebase
- Don't fabricate literals based on plausible-looking names
- If you can't verify, flag uncertainty: "I don't know the exact scope literal — let me check `src/lib/api-types.ts` first" before drafting example

## Anti-patterns to flag immediately

- "Let's build X from scratch" before grep'ing for X
- "Deploy [external platform]" before auditing existing equivalent
- "Add new component Y" alongside existing component Y' that does similar
- "Migration: new table" when existing table can extend
- Writing example SQL / API calls in chat with literals that weren't verified

## Constraints

- Audit-existing applies to YOU when proposing, not just to the user when reviewing.
- "I'll just sketch a quick example" is the most common bypass — sketches in chat are MORE likely to get copy-pasted, not less.
- For external-platform proposals: the cost of ONE 30-minute internal audit is a fraction of the cost of deploying a platform you didn't need.

## Related

- [`08-research-paper-full-read.md`](08-research-paper-full-read.md) — same family: verify source before claim, applied to research papers
- [`07-destructive-action-discipline.md`](07-destructive-action-discipline.md) — same family: verify behavior before suggesting destructive operation
- [`01-claude-md-rules.md`](01-claude-md-rules.md) — never skim read rule (rule #3)
