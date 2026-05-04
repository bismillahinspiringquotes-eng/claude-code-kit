---
name: seo-audit-multilocale
description: Use when auditing a multi-language website (Indonesian + English, plus any other locales) for SEO correctness — hreflang tag setup, sitemap language alternates, canonical URLs, JSON-LD structured data, locale-aware redirects. Most generic SEO checklists ignore multilocale and miss the highest-impact issues. Examples use Next.js + Vercel, but checks apply to any framework that serves multiple language versions.
---

# SEO audit — multilocale website

You're auditing a website that serves content in multiple languages (typically Indonesian + English, sometimes Chinese, Malay, etc.) for the 6 multilocale-specific SEO mistakes that hurt rankings.

Examples use Next.js + Vercel since that's the kit's recommended stack. Same concepts apply to any framework — replace `app/` with the equivalent route structure.

## The 6 checks

### 1. Hreflang tags missing or wrong

**What goes wrong:** Google doesn't know which language version to show in which country → wrong language served to users → bounces + ranking damage.

**What to check:**
- Every page that exists in multiple languages should declare hreflang alternates in metadata.
- For Next.js: in `generateMetadata` or `metadata` export, look for `alternates: { languages: { ... } }`.
- The alternates must be RECIPROCAL: page A says "English version is at /en/page", and the English page must say "Indonesian version is at /id/page" (or whatever the path is).
- Include `x-default` pointing to the fallback locale (usually the most-traffic version).
- Use proper language codes: `id` for Indonesian, `en` for English, `id-ID` only when targeting Indonesia specifically.

**Sample what right looks like:**
```
alternates: {
  canonical: 'https://example.com/id/about',
  languages: {
    'id': 'https://example.com/id/about',
    'en': 'https://example.com/en/about',
    'x-default': 'https://example.com/id/about',
  },
}
```

### 2. Sitemap missing language alternates

**What goes wrong:** sitemap.xml lists pages but doesn't link locale variants → Google indexes them as separate, unrelated pages.

**What to check:**
- Read the sitemap source (Next.js: `app/sitemap.ts`).
- For each multilingual page, the sitemap entry should include `alternates.languages` with all variants.
- Check that BOTH locales appear in the sitemap, not just the default.

**Sample correct entry:**
```
{
  url: 'https://example.com/id/page-1',
  alternates: {
    languages: {
      id: 'https://example.com/id/page-1',
      en: 'https://example.com/en/page-1',
    },
  },
}
```

### 3. Canonical URL inconsistent or self-referential mistake

**What goes wrong:** every page version points canonical to the SAME URL → Google sees them as duplicates and indexes only one. Or every page points canonical to itself → no consolidation signal.

**What to check:**
- For each language variant: canonical should point to ITSELF (the locale version). Not to the default-language version.
- The hreflang and canonical work together: canonical = "this is the authoritative URL for THIS language", hreflang = "here are the other-language equivalents".

### 4. Locale routing trapped behind JS / not server-rendered

**What goes wrong:** locale chosen via client-side JavaScript only → Googlebot sees one default version → other locales never indexed.

**What to check:**
- Are language variants rendered server-side? URL pattern like `/id/`, `/en/`, or `id.example.com`, `en.example.com` — these are crawlable.
- Pure JS toggles (`<button onClick={setLocale('en')}>`) without URL change = invisible to Google.
- For Next.js: i18n routing config or route groups (`(id)`, `(en)`) create proper crawlable URLs.

### 5. Structured data (JSON-LD) missing or wrong language

**What goes wrong:** JSON-LD describes content in default language only → Google doesn't know rich snippets exist for other locales → loses rich-result eligibility.

**What to check:**
- Every public page should have JSON-LD. For each multilingual page, JSON-LD's `inLanguage` field should match the page's locale.
- Different page types need different schema: Article, Product, TouristTrip, Organization, BreadcrumbList.
- Example check: open the rendered HTML for the Indonesian version of an article. Find `<script type="application/ld+json">`. Confirm `"inLanguage": "id"`.
- Common mistake: copy-pasted JSON-LD across locales without updating `inLanguage` and content fields.

### 6. Locale-aware redirects from old URLs missing

**What goes wrong:** site relaunch changed URL structure (e.g. `/about` → `/id/about`). Old URLs return 404 → Google de-indexes → traffic drops.

**What to check:**
- For each old URL pattern, is there a redirect to the locale-aware new URL?
- For Next.js: check `redirects()` in `next.config.mjs`, or middleware-based redirects.
- Use 301 (permanent) for SEO consolidation, not 302 (temporary).
- Detect locale from header (`Accept-Language`) or geo (Vercel geo headers) for the redirect destination, with a stable fallback.

## How to deliver the report

```
# Multilocale SEO audit — [site name]

## Coverage summary
- Pages audited: N
- Locales detected: [id, en, ...]
- Pages missing hreflang: N
- Pages with broken canonical: N

## Critical issues
1. [issue] — Pages affected: [list or count]. Evidence: [file:line]. Fix: [concrete step].

## Warnings
[same format]

## Verified clean
- [check]
```

End with one of:
- "Critical: [N]. Mau gue bantu fix yang #1 dulu (biasanya hreflang reciprocity), atau lo review dulu?"
- "Critical: 0, warnings: [N]. Mau lanjut ke content audit (`/skill seo-content-audit`)?"

## Constraints

- Validate hreflang reciprocity by actually checking BOTH directions — A→B and B→A.
- Don't recommend installing `next-i18next` or any tool unless the codebase actually needs it. Check what's already there.
- For sites with only one language: skip this skill, recommend `seo-content-audit` instead.
