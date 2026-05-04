# Research paper full read

> When citing research papers or external documents where specific numbers / figures matter, NEVER ground-truth claims on AI-summarized text. Read the actual figures + appendix + raw data. Captured from a research misread that took four rounds of correction to fix.

## The rule

When fetching research papers, white papers, technical specifications, or any document where specific numbers / figures matter:

- **Never** ground-truth claims on a WebFetch / AI-summarizer output alone.
- **Always** retrieve the actual figures, charts, methodology section, and appendix.
- For PDFs: use the PDF reader with explicit page range, parse figures and tables directly.
- For web pages: WebFetch is OK for orientation, but verify specific claims against source HTML / figures.
- If unable to retrieve source figures: explicitly flag uncertainty. Do not paper over with AI-summarized confidence.

## Why

Any AI-summarization tool compresses by definition. Summaries:

- May reorder importance (top finding ≠ load-bearing finding)
- May fabricate framing the source didn't use ("red-line" terminology, etc.)
- Miss numerical specificity (charts compressed to "high stakes")
- Miss caveats and limitations sections (often the most important for honest application)
- May confidently report inferences as direct quotes

## Real incident: four rounds of correction

A research paper was being cited. The progression:

- **Round 1:** WebFetch summary said "[topic] grouped with red-line domains" — source actually said "high-stakes" not "red-line".
- **Round 2:** Founder surfaced Figure 1, showing the topic is its own cluster, not grouped.
- **Round 3:** Founder surfaced Figure 2, showing one specific risk dimension was LOW for the topic, not high.
- **Round 4:** Founder surfaced the Limitations section showing the study was process-not-outcome with non-representative sample.
- **Round 5:** Full PDF revealed appendix figures: a different load-bearing risk dimension WAS high (directionally the original framing was right but for different reasons), plus an 11-point framework the body didn't summarize, plus a warning about under-trained domains the summary completely missed.

Each round corrected ONE error introduced by the prior round. The pattern: WebFetch's compression dropped the load-bearing data, the AI-summarizer hallucinated framing, the corrections overshot in the opposite direction. Only full-paper read with figures gave truth.

## How to apply

### Triggers

- User shares a research paper URL → don't WebFetch + summarize. Fetch + read figures + extract numbers verbatim. Quote.
- User cites a study or statistic → grep for the actual claim in the actual paper. If you can't access the source, say so explicitly.
- About to recommend action based on research finding → verify the finding against source figures before recommending.

### Concrete process for research papers

1. WebFetch only for initial orientation (URLs, structure)
2. Identify all figures, charts, appendices mentioned
3. Read each figure's data verbatim — extract numbers
4. Read methodology + limitations sections in full
5. Cross-reference any claim made in body text against figure data
6. If quoting paper, use direct quote with page reference
7. Report what's UNCLEAR or ABSENT from the paper, not just what's present

### Anti-patterns to avoid

- "The paper says X" without quoting X
- Paraphrasing percentages (always use exact numbers from charts)
- Treating an AI-summarizer's framing as authoritative
- Stopping at one WebFetch when source has multiple pages / figures
- Confident statements that overshoot in the correction direction (correcting from "X is high" to "X is low" without checking which dimension you're talking about)

## Constraints

- Research papers are particularly load-bearing because they get cited as ground truth in subsequent work. A wrong paraphrase propagates. A correct quote does not.
- "Just give me the gist" requests from the user are NOT permission to skip the figures. The gist is downstream of the figures, not a substitute for them.
- For papers behind paywalls or in formats your tools can't parse (e.g., scanned PDFs without OCR): flag the limitation, don't simulate access.

## Related

- [`06-audit-existing-before-build.md`](06-audit-existing-before-build.md) — same family of "verify source before claim", applied to existing codebase
- [`04-deploy-verification.md`](04-deploy-verification.md) — same pattern in production: verify at the live URL, not at the summary layer
- [`01-claude-md-rules.md`](01-claude-md-rules.md) — never-skim-read rule (rule #3) applies to research papers same as code
