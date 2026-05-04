---
name: seo-content-audit
description: Use when auditing a website's content/markup for on-page SEO basics — title tags, meta descriptions, heading hierarchy, image alt text, internal linking, structured data presence, sitemap correctness. Stack-agnostic; examples reference common Next.js + Vercel patterns but apply to any web framework.
---

# SEO audit — on-page content

You're auditing a website for the 7 most common on-page SEO mistakes. These directly affect rankings and click-through rate.

For multilocale-specific issues (hreflang, language alternates), use the `seo-audit-multilocale` skill instead — this one assumes single-language or covers single-language aspects.

## The 7 checks

### 1. Title tag — missing, wrong length, or generic

**What to check:**
- Every public page has a title tag (visible in browser tab + Google search result).
- Length: 50-60 characters ideal. Under 30 = wasted opportunity. Over 70 = truncated in search results.
- Each page has a UNIQUE title. Grep all `title:` in metadata files — duplicates = SEO confusion.
- Includes the primary keyword for that page (not stuffed, just present).
- For Next.js: check `metadata` exports or `generateMetadata` functions in route files.

### 2. Meta description — missing or generic

**What to check:**
- Every public page has a meta description.
- Length: 140-160 characters. Under 100 = thin. Over 170 = truncated.
- Each page has a UNIQUE description.
- It actually describes the page content (not the site tagline copy-pasted everywhere).
- Includes a soft call-to-action when appropriate ("Learn how to...", "Discover...", "Book now").

### 3. Heading hierarchy broken

**What to check:**
- Each page has exactly ONE `<h1>`. Grep template files; verify.
- Heading order makes sense: `h1 > h2 > h3` — don't jump from `h1` straight to `h4`.
- The `h1` matches user search intent and contains the primary keyword.
- No use of headings for visual styling (e.g. `h3` for a small bold sentence) — use CSS for that.

### 4. Image alt text missing or auto-generated junk

**What to check:**
- Every `<img>` or `<Image>` tag has a meaningful `alt` attribute.
- Alt text describes what's IN the image, in context. "boat-1.jpg" or "" = useless.
- Decorative images (purely visual flourish) should have `alt=""` (empty), not be missing entirely.
- For Next.js: every `next/image` import should have `alt` prop set per usage.

### 5. Internal linking sparse or broken

**What to check:**
- Each public page has at least 2-3 internal links to related content.
- Anchor text is descriptive ("see our Komodo guide"), not generic ("click here", "read more").
- Run a quick crawl: are there pages that no other page links to (orphan pages)? Those rank poorly.
- Are there broken internal links (pointing to deleted pages)? Test a sample.

### 6. Structured data (JSON-LD) missing for content types

**What to check:**
- For pages of type Article: JSON-LD with `@type: "Article"`, including author, datePublished, headline, image.
- For pages of type Product / Tour / Service: corresponding schema.
- For homepage: `Organization` schema with logo, contact, social.
- For navigation breadcrumbs: `BreadcrumbList` schema.
- Validate at https://search.google.com/test/rich-results for any sample page.

### 7. Sitemap.xml + robots.txt issues

**What to check:**
- Sitemap exists at `/sitemap.xml`. For Next.js: usually `app/sitemap.ts`.
- Sitemap includes ALL public pages, not just static ones — dynamic routes (articles, products) must be listed too.
- Sitemap entries have `lastModified` dates that update when content changes.
- robots.txt at `/robots.txt`. For Next.js: `app/robots.ts`.
- robots.txt does NOT block important pages. Common mistake: leftover `Disallow: /` from staging deployment.
- Sitemap URL is referenced from robots.txt: `Sitemap: https://example.com/sitemap.xml`.

## How to deliver the report

```
# On-page SEO audit — [site name]

## Coverage
- Pages audited: N
- Pages with missing title: N
- Pages with missing meta description: N
- Pages with broken heading hierarchy: N
- Images without alt text: N

## Critical issues
1. [issue] — Pages affected: [list or count]. Evidence: [file:line]. Fix: [concrete step].

## Warnings
[same format]

## Verified clean
- [check]
```

End with: "Critical: [N], warnings: [N]. Mau gue bantu fix yang highest-impact (biasanya title tags + JSON-LD), atau lo review dulu?"

## Constraints

- Don't recommend installing SEO plugins (Yoast, RankMath) — those are WordPress-specific. The kit's audience uses code-first frameworks.
- Don't dump generic "SEO best practices" — only call out what's actually wrong in THIS codebase.
- If audit takes more than 10 minutes of file reading, sample a representative subset (5-10 pages across page types) and note that in the report.
- Cite file paths + line numbers for every finding.
