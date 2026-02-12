# Story 1.5.2: SEO & Analytics Foundation

**Status:** done

## Story

As a **product owner**,
I want **proper SEO and privacy-first analytics on the landing page**,
so that **I can track user acquisition and optimize for search engine discovery**.

## Acceptance Criteria

1. **AC9:** Open Graph tags present
   - `og:title` meta tag set to "IHSPAX - Seek and Find Catholic Audio"
   - `og:description` meta tag set to value proposition summary
   - `og:image` meta tag pointing to a social sharing image (1200x630px)
   - `og:url` meta tag set to `https://ihspax.com`
   - `og:type` set to "website"

2. **AC10:** JSON-LD structured data
   - `<script type="application/ld+json">` present in page head
   - Contains valid Organization schema with name "IHSPAX"
   - Contains valid WebApplication schema with app store links
   - Validates against Schema.org specifications

3. **AC11:** sitemap.xml exists
   - GET `/sitemap.xml` returns valid XML sitemap
   - Sitemap includes the landing page URL (`https://ihspax.com`)
   - Generated at build time (compatible with static export)

4. **AC12:** robots.txt exists
   - GET `/robots.txt` returns valid robots.txt
   - Allows all crawlers to access the site
   - References sitemap.xml location
   - Generated at build time (compatible with static export)

5. **AC13:** Analytics tracking active
   - Privacy-first analytics script loads on page
   - Page views are tracked without cookies
   - GDPR-compliant (no personal data collection)
   - Analytics script does not block page rendering

6. **AC14:** No Google Analytics
   - No `gtag.js`, `analytics.js`, or Google tracking scripts present in page source
   - No Google-related tracking cookies set
   - Verified by test scanning rendered HTML

## Tasks / Subtasks

- [x] **Task 1: Add Open Graph meta tags** (AC: 9)
  - [x] 1.1: Extend `layout.tsx` metadata export with `openGraph` configuration
  - [x] 1.2: Create placeholder OG image (1200x630px) at `public/og-image.png`
  - [x] 1.3: Add Twitter Card meta tags (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`)

- [x] **Task 2: Add JSON-LD structured data** (AC: 10)
  - [x] 2.1: Create JSON-LD script with Organization schema in `layout.tsx`
  - [x] 2.2: Add WebApplication schema with app store links
  - [x] 2.3: Validate JSON-LD output via unit tests

- [x] **Task 3: Generate sitemap.xml** (AC: 11)
  - [x] 3.1: Create `app/sitemap.ts` using Next.js Metadata API
  - [x] 3.2: Include landing page URL in sitemap
  - [x] 3.3: Verify sitemap is generated in static export `out/` directory

- [x] **Task 4: Generate robots.txt** (AC: 12)
  - [x] 4.1: Create `app/robots.ts` using Next.js Metadata API
  - [x] 4.2: Allow all crawlers, reference sitemap URL
  - [x] 4.3: Verify robots.txt is generated in static export `out/` directory

- [x] **Task 5: Integrate privacy-first analytics** (AC: 13, 14)
  - [x] 5.1: Add Plausible Analytics via Next.js `Script` component (strategy: afterInteractive)
  - [x] 5.2: Configure data-domain for `ihspax.com`
  - [x] 5.3: Script uses `defer` attribute (non-render-blocking)
  - [x] 5.4: Verified no Google Analytics scripts present (test coverage)

- [x] **Task 6: Write SEO and analytics tests** (AC: 9, 10, 11, 12, 13, 14)
  - [x] 6.1: Test: Open Graph meta tags present in layout metadata export (5 tests)
  - [x] 6.2: Test: JSON-LD contains valid Organization and WebApplication schemas (6 tests)
  - [x] 6.3: Test: sitemap.ts returns valid sitemap entries (1 test)
  - [x] 6.4: Test: robots.ts returns valid robots configuration (2 tests)
  - [x] 6.5: Test: No Google Analytics script references (1 test)
  - [x] 6.6: Typecheck, lint, and build all pass — sitemap.xml + robots.txt confirmed in `out/`

## Dev Notes

### Architecture Context

Per `docs/architecture.md` (B-Lite Hybrid Strategy):

- **Static Export:** `apps/public/` uses `output: "export"` — all files must be pre-renderable
- **Next.js Metadata API:** Use App Router metadata conventions (`sitemap.ts`, `robots.ts`, `opengraph-image.*`)
- **No API routes:** Static export doesn't support server-side API routes — use metadata file conventions instead
- **Analytics:** FR57 mandates no advertising tracking — Plausible Analytics is privacy-first, cookie-free

### Technical Decisions

| Decision | Value | Source |
|----------|-------|--------|
| Analytics Provider | Plausible Analytics (script tag) | Tech spec Q1 default, FR57 compliance |
| OG Image | Static placeholder PNG (1200x630) | Standard social sharing dimensions |
| Sitemap Generation | Next.js `app/sitemap.ts` | App Router metadata convention |
| Robots.txt | Next.js `app/robots.ts` | App Router metadata convention |
| JSON-LD | Inline `<script>` in layout | SEO best practice for structured data |
| Twitter Cards | `summary_large_image` | Maximizes social sharing visibility |

### Static Export Considerations

Next.js `output: "export"` supports metadata file conventions:
- `app/sitemap.ts` → generates `sitemap.xml` in `out/`
- `app/robots.ts` → generates `robots.txt` in `out/`
- `layout.tsx` metadata export → generates `<meta>` tags in HTML

These all work because they're evaluated at build time, not runtime.

### Analytics Implementation Notes

Plausible Analytics via script tag (no npm package needed):
```html
<script defer data-domain="ihspax.com" src="https://plausible.io/js/script.js"></script>
```
- No cookies, GDPR-compliant by default
- `defer` attribute ensures non-render-blocking
- Tracks page views automatically
- Event tracking available via `plausible()` function (for Story 1-5-3 CTA tracking)

### Project Structure Notes

Files to create/modify (all under `apps/public/`):
```
apps/public/
├── app/
│   ├── layout.tsx          # MODIFY: Add OG, JSON-LD, analytics script
│   ├── sitemap.ts          # NEW: Sitemap generation
│   └── robots.ts           # NEW: Robots.txt generation
├── public/
│   └── og-image.png        # NEW: Social sharing image placeholder
```

### References

- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Story-1-5-2]
- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Acceptance-Criteria]
- [Source: docs/epics.md#Story-1.5.2:-SEO-&-Analytics-Foundation]
- [Source: docs/architecture.md#Web-Strategy:-B-Lite-Architecture]
- [Source: docs/prd.md#FR57] (no advertising tracking)

### Learnings from Previous Story

**From Story 1-5-1-landing-page-foundation (Status: done)**

- **Layout metadata pattern:** `layout.tsx` already exports `metadata: Metadata` and `viewport: Viewport` separately — extend metadata with `openGraph` and `twitter` fields
- **Viewport separated from Metadata:** Next.js 14+ requires `Viewport` as a separate export — don't add viewport-related fields to `metadata`
- **ESM compatibility:** Use `fileURLToPath(import.meta.url)` instead of `__dirname` in any config files
- **Static export confirmed working:** `output: "export"` generates `out/` directory — verify sitemap.xml and robots.txt appear there
- **Existing test infrastructure:** 14 tests in `components/__tests__/` and `app/__tests__/` — add SEO tests following same patterns
- **Root vitest uses `test.projects`:** New test files in `apps/public/` will be auto-discovered
- **eslint.config.mjs:** Already configured with `eslint-config-next` (core-web-vitals + typescript)

[Source: docs/sprint-artifacts/1-5-1-landing-page-foundation.md#Dev-Agent-Record]

## Dev Agent Record

### Context Reference

- `docs/sprint-artifacts/tech-spec-epic-1-5.md`
- `docs/sprint-artifacts/1-5-1-landing-page-foundation.md` (previous story learnings)

### Agent Model Used

Claude Opus 4.6

### Debug Log References

- `sitemap.ts` and `robots.ts` required `export const dynamic = "force-static"` for Next.js 16 `output: "export"` compatibility — build failed without it
- JSON-LD tests initially tried rendering `<html>` inside jsdom container, causing script tag not found — refactored to export `jsonLd` object and test data directly
- OG image created as minimal valid PNG (3.6KB, 1200x630, teal #2c4a62 background) — placeholder for real design asset
- Plausible Analytics uses `next/script` with `strategy="afterInteractive"` for proper Next.js integration

### Completion Notes List

- All 6 task groups completed with all subtasks
- All 6 acceptance criteria met (AC9-AC14)
- 17 new tests passing across 2 test files (seo.test.tsx: 11, jsonld.test.tsx: 6)
- Total test count: 31 (17 new + 14 from Story 1-5-1)
- Typecheck, lint, and static build all pass
- Static export confirmed: `sitemap.xml`, `robots.txt`, and `og-image.png` all present in `out/`
- `jsonLd` exported from layout for testability and reusability
- Plausible script uses Next.js `Script` component with `afterInteractive` strategy
- Twitter Cards configured as `summary_large_image` for maximum social sharing impact

### File List

**New files (in `apps/public/`):**
- `app/sitemap.ts` — sitemap generation with `force-static` for SSG
- `app/robots.ts` — robots.txt generation with `force-static` for SSG
- `app/__tests__/seo.test.tsx` — 11 tests (OG, Twitter, sitemap, robots, no-GA)
- `app/__tests__/jsonld.test.tsx` — 6 tests (Organization + WebApplication schemas)
- `public/og-image.png` — placeholder OG image (1200x630, teal background)

**Modified files (in `apps/public/`):**
- `app/layout.tsx` — added openGraph, twitter, jsonLd (exported), Plausible `Script`

## Change Log

| Date | Change | Author |
|------|--------|--------|
| 2026-02-12 | Story drafted from tech spec and epics | SM Agent (Claude Opus 4.6) |
| 2026-02-12 | Implementation complete — all tasks, tests, and ACs done | Dev Agent (Claude Opus 4.6) |
