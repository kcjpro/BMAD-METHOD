# Story 1.5.3: App Launch Integration

**Status:** done

## Story

As a **visitor**,
I want **seamless transition from the landing page to the app**,
so that **I can start using IHSPAX on my preferred platform immediately**.

## Acceptance Criteria

1. **AC15:** Desktop CTA routes to web app
   - Clicking "Launch App" on desktop navigates to `https://app.ihspax.com`
   - Link opens in the same tab (direct navigation)

2. **AC16:** iOS download routes to App Store
   - Clicking iOS badge navigates to App Store listing
   - App Store URL: `https://apps.apple.com/app/ihspax`

3. **AC17:** Android download routes to Play Store
   - Clicking Android badge navigates to Google Play listing
   - Play Store URL: `https://play.google.com/store/apps/details?id=com.ihspax`

4. **AC18:** CTA clicks tracked in analytics
   - Analytics records custom events for each CTA click type
   - Event names: `CTA: Launch App`, `CTA: Download iOS`, `CTA: Download Android`, `CTA: Download Mobile`
   - Events fire before navigation occurs
   - Plausible custom events API used (`window.plausible()`)

## Tasks / Subtasks

- [x] **Task 1: Create analytics event tracking utility** (AC: 18)
  - [x] 1.1: Create `lib/analytics.ts` with `trackEvent()` helper function
  - [x] 1.2: Type-safe event names for CTA tracking
  - [x] 1.3: Graceful fallback when Plausible is not loaded (dev/test)

- [x] **Task 2: Add click tracking to CTAButton** (AC: 15, 18)
  - [x] 2.1: Add optional `eventName` prop to CTAButton
  - [x] 2.2: Fire analytics event on click before navigation
  - [x] 2.3: Mark CTAButton as client component (`"use client"`)

- [x] **Task 3: Add click tracking to AppStoreBadges** (AC: 16, 17, 18)
  - [x] 3.1: Inline click handlers instead of TrackedLink wrapper (simpler approach)
  - [x] 3.2: Wire App Store badge click to `CTA: Download iOS` event
  - [x] 3.3: Wire Google Play badge click to `CTA: Download Android` event
  - [x] 3.4: Mark AppStoreBadges as client component (`"use client"`)

- [x] **Task 4: Update Hero with tracked CTAs** (AC: 15, 18)
  - [x] 4.1: Add `eventName` props to Hero CTAButtons
  - [x] 4.2: "Launch App" fires `CTA: Launch App` event
  - [x] 4.3: "Download for Mobile" fires `CTA: Download Mobile` event

- [x] **Task 5: Write tests** (AC: 15, 16, 17, 18)
  - [x] 5.1: Test: CTAButton fires analytics event on click
  - [x] 5.2: Test: AppStoreBadges links have correct hrefs
  - [x] 5.3: Test: Hero CTAButtons have correct hrefs and event tracking
  - [x] 5.4: Test: Analytics trackEvent handles missing plausible gracefully
  - [x] 5.5: Verify typecheck, lint, and build pass

## Dev Notes

### Architecture Context

Per `docs/architecture.md` (B-Lite Hybrid Strategy):

- **Domain routing:** `ihspax.com` (landing) → `app.ihspax.com` (Expo Web) → App Store / Play Store
- **Static site:** All analytics tracking must work client-side (no server components for click handlers)
- **Plausible events:** Custom events via `window.plausible()` function (loaded in Story 1-5-2)

### Technical Decisions

| Decision | Value | Source |
|----------|-------|--------|
| Event tracking | Plausible custom events (`window.plausible()`) | Tech spec observability section |
| Client components | Only components with click handlers need `"use client"` | Next.js App Router convention |
| Analytics utility | Shared `lib/analytics.ts` | Reusable for future event tracking |
| Link behavior | Same-tab navigation (no `target="_blank"`) | Standard CTA UX pattern |

### Plausible Custom Events

Plausible's script tag (loaded in layout.tsx from Story 1-5-2) exposes `window.plausible()`:
```typescript
// Type declaration
declare global {
  interface Window {
    plausible?: (event: string, options?: { props?: Record<string, string> }) => void;
  }
}

// Usage
window.plausible?.("CTA: Launch App");
```
- Events appear in Plausible dashboard under "Goal Conversions"
- No additional configuration needed — events auto-register on first fire

### Project Structure Notes

Files to create/modify (all under `apps/public/`):
```
apps/public/
├── lib/
│   └── analytics.ts            # NEW: Event tracking utility
├── components/
│   ├── CTAButton.tsx            # MODIFY: Add eventName prop + click handler
│   ├── AppStoreBadges.tsx       # MODIFY: Add click tracking
│   ├── Hero.tsx                 # MODIFY: Pass eventName to CTAButtons
│   └── __tests__/
│       └── analytics.test.ts   # NEW: Analytics utility tests
```

### References

- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Story-1-5-3]
- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Acceptance-Criteria]
- [Source: docs/epics.md#Story-1.5.3:-App-Launch-Integration]
- [Source: docs/architecture.md#Web-Strategy:-B-Lite-Architecture]
- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Observability]

### Learnings from Previous Story

**From Story 1-5-2-seo-analytics-foundation (Status: done)**

- **Plausible loaded via `next/script`:** Analytics script in layout.tsx with `strategy="afterInteractive"` — `window.plausible` available after hydration
- **`force-static` required:** Any new route files for static export need `export const dynamic = "force-static"`
- **JSON-LD exported for testability:** Pattern of exporting data objects from layout for direct unit testing — reuse for analytics config if needed
- **Test pattern:** SEO tests import metadata/sitemap/robots directly and test data — same pattern for analytics utility
- **31 tests passing:** Add to existing test suite in `components/__tests__/` and `app/__tests__/`

[Source: docs/sprint-artifacts/1-5-2-seo-analytics-foundation.md#Dev-Agent-Record]

## Dev Agent Record

### Context Reference

Story drafted inline from tech spec — no separate context XML generated (story was simple enough to implement directly).

### Agent Model Used

Claude Opus 4.6

### Debug Log References

- TypeScript error: `window as Record<string, unknown>` cast insufficient — fixed with double cast `window as unknown as Record<string, unknown>` in tests
- Task 3.1 deviation: Used inline `onClick` handlers on badge links instead of creating a separate `TrackedLink` wrapper component — simpler approach for only 2 tracked links

### Completion Notes List

- Created `lib/analytics.ts` with type-safe `CTAEvent` union type and `trackEvent()` helper
- `trackEvent()` gracefully no-ops when `window.plausible` is undefined (dev/test environments)
- CTAButton converted to client component with optional `eventName` prop — fires analytics before navigation
- AppStoreBadges converted to client component with inline `onClick` handlers for iOS/Android tracking
- Hero passes `eventName` props to both CTAButton instances
- 10 new tests added (4 analytics utility + 2 CTAButton analytics + 2 Hero analytics + 2 AppStoreBadges analytics)
- All 41 tests pass, typecheck clean, lint clean, build succeeds with static export

### File List

| File | Action | Description |
|------|--------|-------------|
| `apps/public/lib/analytics.ts` | Created | Event tracking utility with `trackEvent()` and `CTAEvent` type |
| `apps/public/components/CTAButton.tsx` | Modified | Added `"use client"`, `eventName` prop, click handler |
| `apps/public/components/Hero.tsx` | Modified | Added `eventName` props to CTAButton instances |
| `apps/public/components/AppStoreBadges.tsx` | Modified | Added `"use client"`, inline click tracking |
| `apps/public/lib/__tests__/analytics.test.ts` | Created | 4 tests for trackEvent utility |
| `apps/public/components/__tests__/CTAButton.test.tsx` | Modified | Added 2 analytics event tests |
| `apps/public/components/__tests__/Hero.test.tsx` | Modified | Added 2 analytics event tests |
| `apps/public/components/__tests__/AppStoreBadges.test.tsx` | Modified | Added 2 analytics event tests |

## Change Log

| Date | Change | Author |
|------|--------|--------|
| 2026-02-12 | Story drafted from tech spec and epics | SM Agent (Claude Opus 4.6) |
| 2026-02-12 | Implementation complete — all ACs verified, 41 tests pass | Dev Agent (Claude Opus 4.6) |
