# Contributing to claude-code-kit

## What this kit accepts

Patterns that meet ALL three criteria:

1. **Real production-tested.** The pattern saved you from a specific incident, or you've used it across 2+ production repos. Hypothetical "this might be useful" patterns get closed.
2. **Sanitized.** No real IPs, project IDs, customer data, founder contact details, internal team names, or specific incident identifiers. Generalize the war story without losing the lesson.
3. **Includes WHY + HOW TO APPLY.** Every rule needs a one-line `Why:` (the incident or pain that motivated it) and a one-line `How to apply:` (when this kicks in). Rules without WHY rot — future maintainers can't judge edge cases.

## How to propose a pattern

**Discussion-first.** Open an issue describing:

- The pain or incident the pattern addresses
- The pattern itself (rough form is fine)
- Where you think it fits (existing doc, or new file)

We discuss in the issue. If we agree it fits, you draft the PR. This avoids the "wrote 800 words, got rejected" problem.

## PR checklist

- [ ] Pattern came from a real incident or 2+ repo experience (state which in PR description)
- [ ] Sanitized: no leaked IDs, customer data, internal names, real contact info
- [ ] Includes `Why:` + `How to apply:` lines on each new rule
- [ ] Fits an existing doc, OR proposes a new doc with rationale
- [ ] No new top-level files without prior issue discussion
- [ ] If adding a template, includes a placeholder example
- [ ] If adding to `docs/`, follows the numbered prefix convention (see `docs/INDEX.md`)
- [ ] Sanitization grep clean: no IPs, no API keys, no internal team names, no real emails

## Voice + format rules

- Indonesian casual is fine for in-doc voice (the kit's differentiator). Don't translate `gue/lo` to "I/you" in template content — that destroys the register that makes the kit useful.
- Code blocks > prose for rules. Tables > prose for diagnostic matrices.
- Section headers should answer a question ("Why this matters", not "Background").
- Keep individual docs under 1500 words. Split into 0Xa / 0Xb if longer.

## What we won't accept

- Patterns ported from other frameworks/tools without production use here
- Generic "best practices" lists (Google has those)
- Toy examples or hello-world demos
- Closed-source tool recommendations as primary path (open-source first; closed-source can be a footnote)
- Patterns that contradict `docs/00-philosophy.md` without arguing why the philosophy should change

## Out of scope

- Code changes to a starter or boilerplate (this kit sits ABOVE boilerplates, not as one)
- Marketing copy, brand assets, demos
- Translations of existing docs (the bilingual register is intentional)

## License

By submitting a PR, you agree your contribution is MIT-licensed.
