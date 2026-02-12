# Story 1.5.1: Landing Page Foundation

**Status:** done

## Story

As a **visitor**,
I want **a professional landing page at ihspax.com**,
so that **I understand what IHSPAX offers and trust the platform**.

## Acceptance Criteria

1. **AC1:** Hero section displays value proposition
   - Page contains "Seek and Find Catholic Audio" text in hero section
   - IHSPAX logo visible in header area
   - Primary CTA button visible above the fold

2. **AC2:** Professional imagery reflects Sanctuary Serenity aesthetic
   - Visual design uses `#2c4a62` (teal) primary and `#e6c866` (gold) accent colors
   - Dark teal header consistent with UX Design Spec Direction 7
   - Warm cream background (`#f0ebe3`) for content areas
   - System fonts for native feel and performance

3. **AC3:** Clear CTA present
   - "Launch App" or "Download" button is visible above the fold
   - CTA uses gold accent color (`#e6c866`) per button hierarchy (primary)
   - Links to placeholder URLs (App Store, Play Store, or web app)

4. **AC4:** Feature highlights visible
   - Three feature cards displayed: Semantic Search, Daily Meditations, Offline Listening
   - Each card has icon, title, and brief description
   - Cards use surface color (`#faf8f5`) with subtle shadow

5. **AC5:** App Store badges present
   - Apple App Store badge displayed with link (placeholder URL acceptable)
   - Google Play badge displayed with link (placeholder URL acceptable)

6. **AC6:** Page loads within 2 seconds
   - Static generation (SSG) used — pre-rendered at build time
   - Lighthouse Performance score > 90
   - Total page weight < 500KB initial load
   - Images optimized via `next/image` with WebP format

7. **AC7:** Fully responsive
   - Layout adapts correctly at mobile (<768px), tablet (768-1024px), and desktop (>1024px)
   - Touch targets minimum 44x44px on mobile
   - No horizontal scroll at any breakpoint

8. **AC8:** SEO meta tags configured
   - `<title>` tag present with "IHSPAX - Seek and Find Catholic Audio"
   - `<meta name="description">` present with value proposition
   - Viewport meta tag configured for responsive design

## Tasks / Subtasks

- [x] **Task 1: Create `apps/public/` Next.js application** (AC: 1, 6)
  - [x] 1.1: Initialize Next.js app with App Router in `apps/public/`
  - [x] 1.2: Configure `package.json` with name "public", scripts (dev, build, start, lint, typecheck)
  - [x] 1.3: Configure `tsconfig.json` matching admin app patterns
  - [x] 1.4: Configure `next.config.ts` with static export settings
  - [x] 1.5: Add root `package.json` scripts: `public:dev`, `public:build`
  - [x] 1.6: Verify `pnpm-workspace.yaml` already includes `apps/*` (confirmed auto-discovery)
  - [x] 1.7: Run `pnpm install` to link workspace

- [x] **Task 2: Configure Tailwind CSS with Sanctuary theme** (AC: 2)
  - [x] 2.1: Install and configure Tailwind CSS 4 with PostCSS
  - [x] 2.2: Define color tokens matching UX Design Spec:
    - Primary: `#2c4a62`, Accent: `#e6c866`, Background: `#f0ebe3`, Surface: `#faf8f5`
    - Text primary: `#3d4852`, Text secondary: `#5a6570`, Border: `#ddd5c8`
  - [x] 2.3: Configure spacing scale (4px base unit), border radius, and shadow tokens
  - [x] 2.4: Set up global CSS with base styles and font stack

- [x] **Task 3: Build landing page components** (AC: 1, 3, 4, 5)
  - [x] 3.1: Create `Hero` component — logo, headline, subheadline, primary CTA
  - [x] 3.2: Create `FeatureHighlights` component — three feature cards with icons
  - [x] 3.3: Create `TrustSignals` component — speaker names, partnership mentions
  - [x] 3.4: Create `AppStoreBadges` component — App Store + Google Play download links
  - [x] 3.5: Create `CTAButton` component — styled button with primary/secondary variants
  - [x] 3.6: Create `Footer` component — copyright, links

- [x] **Task 4: Compose landing page** (AC: 1, 2, 3, 4, 5, 8)
  - [x] 4.1: Create `app/layout.tsx` with metadata, fonts, and HTML structure
  - [x] 4.2: Create `app/page.tsx` composing all components
  - [x] 4.3: Add SEO meta tags (`<title>`, `<meta description>`, viewport)
  - [x] 4.4: Add favicon and basic static assets to `public/` directory

- [x] **Task 5: Implement responsive design** (AC: 7)
  - [x] 5.1: Mobile layout (<768px) — single column, stacked components
  - [x] 5.2: Tablet layout (768-1024px) — wider content area, 2-column feature cards
  - [x] 5.3: Desktop layout (>1024px) — max-width container, 3-column feature cards
  - [x] 5.4: Verify no horizontal scroll at all breakpoints
  - [x] 5.5: Verify touch targets ≥ 44x44px on mobile

- [x] **Task 6: Add to CI pipeline and verify build** (AC: 6)
  - [x] 6.1: Add `pnpm --filter public build` step to `.github/workflows/ci.yml`
  - [x] 6.2: Add `typecheck` script to `apps/public/package.json`
  - [x] 6.3: Verify `pnpm typecheck` includes public app — passed
  - [x] 6.4: Verify `pnpm lint` includes public app — passed (with eslint.config.mjs)
  - [x] 6.5: Verify `pnpm test` passes — 14 tests passing
  - [x] 6.6: Verify `pnpm --filter public build` succeeds — static export generated

- [x] **Task 7: Write component tests** (AC: 1, 3, 4, 5, 8)
  - [x] 7.1: Install `@testing-library/react` and `jsdom` for public app
  - [x] 7.2: Create Vitest config for public app (`apps/public/vitest.config.ts`)
  - [x] 7.3: Write test: Hero renders value proposition text (3 tests)
  - [x] 7.4: Write test: FeatureHighlights renders three feature cards (3 tests)
  - [x] 7.5: Write test: AppStoreBadges renders both store links (3 tests)
  - [x] 7.6: Write test: CTAButton renders with correct styling (3 tests)
  - [x] 7.7: Write test: Page renders complete landing page with all sections (2 tests)

## Dev Notes

### Architecture Context

Per `docs/architecture.md` (B-Lite Hybrid Strategy):

- **New App:** `apps/public/` — Next.js with SSG for marketing/SEO
- **Domain:** `ihspax.com` (public landing), separate from `app.ihspax.com` (Expo Web)
- **Styling:** Tailwind CSS (consistent with `apps/admin/`)
- **Deployment:** Vercel (same platform as admin, separate project)
- **No Supabase dependency** — pure static site for MVP

### Technical Decisions

| Decision | Value | Source |
|----------|-------|--------|
| Framework | Next.js with App Router | Architecture doc (B-Lite) |
| Rendering | Static Generation (SSG) | Tech spec performance requirements |
| Styling | Tailwind CSS 4 | Architecture doc (consistent with admin) |
| Next.js Version | Match admin (currently 16) | Tech spec Q3 default |
| Fonts | System fonts | UX spec §3.2 (performance + native feel) |
| Images | `next/image` with WebP | Tech spec performance section |

### Design Reference

Per `docs/ux-design-specification.md` (Direction 7: Sanctuary Serenity):

**Color Tokens (Light Mode):**
| Token | Hex | Usage |
|-------|-----|-------|
| Primary | `#2c4a62` | Header, navigation |
| Primary hover | `#3d6b8c` | Interactive states |
| Accent | `#e6c866` | CTAs, highlights |
| Background | `#f0ebe3` | Page background |
| Surface | `#faf8f5` | Cards, content areas |
| Text primary | `#3d4852` | Body text |
| Text secondary | `#5a6570` | Captions |
| Border | `#ddd5c8` | Subtle borders |

**Key Design Principles:**
- Dark teal header in both light and dark modes (visual anchor)
- Gold gradient logo text on dark header
- Warm cream background — "feels like a Catholic bookstore, not a tech startup"
- Minimum 44x44px touch targets
- Transitions: 0.3s ease

### Project Structure Notes

New files to create (all under `apps/public/`):
```
apps/public/
├── app/
│   ├── layout.tsx         # Root layout with metadata
│   └── page.tsx           # Landing page
├── components/
│   ├── Hero.tsx
│   ├── FeatureHighlights.tsx
│   ├── TrustSignals.tsx
│   ├── AppStoreBadges.tsx
│   ├── CTAButton.tsx
│   └── Footer.tsx
├── public/
│   └── favicon.ico
├── next.config.ts
├── package.json
├── tailwind.config.ts
├── postcss.config.mjs
├── tsconfig.json
└── vitest.config.ts
```

Existing files to modify:
- `package.json` (root) — Add `public:dev` and `public:build` scripts
- `.github/workflows/ci.yml` — Add public build step

### References

- [Source: docs/architecture.md#Web-Strategy:-B-Lite-Architecture]
- [Source: docs/ux-design-specification.md#3.1-Color-System]
- [Source: docs/ux-design-specification.md#4.1-Chosen-Design-Approach]
- [Source: docs/sprint-artifacts/tech-spec-epic-1-5.md#Story-1-5-1]
- [Source: docs/epics.md#Story-1.5.1:-Landing-Page-Foundation]
- [Source: docs/vercel-domain-routing.md]
- [Source: docs/sprint-artifacts/epic-1-5-design-assets.md]

### Learnings from Previous Story

**From Story 1-5-development-environment-documentation (Status: done)**

- **README patterns:** README already contains CI/CD, testing, environment variable documentation — reference for public app README needs
- **Monorepo workspace:** `pnpm-workspace.yaml` includes `apps/*` — new `apps/public/` will be auto-discovered
- **CI pipeline:** `.github/workflows/ci.yml` runs typecheck, lint, test, and admin build — extend with public build
- **Vitest setup:** Vitest added at workspace root with `passWithNoTests: true` — public app can add its own config
- **Admin app patterns:** `apps/admin/` uses Next.js 16, Tailwind 4, `@tailwindcss/postcss` — follow same patterns

[Source: docs/sprint-artifacts/1-5-development-environment-documentation.md#Dev-Agent-Record]

## Dev Agent Record

### Context Reference

- `docs/sprint-artifacts/tech-spec-epic-1-5.md`
- `docs/ux-design-specification.md` (Direction 7: Sanctuary Serenity)
- `docs/architecture.md` (B-Lite Web Strategy)

### Agent Model Used

Claude Opus 4.6

### Debug Log References

- Fixed vitest.config.ts: removed erroneous `react` import from `next/dist/compiled/react`
- Fixed vitest.config.ts: replaced `__dirname` with `fileURLToPath(import.meta.url)` for ESM compat
- Fixed layout.tsx: separated `viewport` from `Metadata` into standalone `Viewport` export (Next.js 14+ API)
- Created `eslint.config.mjs` matching admin app patterns to resolve `no-undef` lint errors
- Configured vitest workspace via `test.projects` in root config (not deprecated `vitest.workspace.ts`)

### Completion Notes List

- All 7 task groups completed with all subtasks
- All 8 acceptance criteria met
- 14 component tests passing across 5 test files
- Typecheck, lint, and static build all pass
- Static export generates `out/` directory with pre-rendered HTML
- ESLint config matches admin app patterns (`eslint-config-next` with core-web-vitals + typescript)
- Root vitest uses `test.projects` to delegate to app-specific configs

### File List

**New files (in `apps/public/`):**
- `package.json` — workspace package with Next.js 16, Tailwind 4, testing deps
- `tsconfig.json` — TypeScript config matching admin app
- `next.config.ts` — static export (`output: "export"`)
- `postcss.config.mjs` — `@tailwindcss/postcss` plugin
- `vitest.config.ts` — jsdom environment, `@` path alias
- `eslint.config.mjs` — ESLint flat config with Next.js rules
- `app/globals.css` — Tailwind import + Sanctuary Serenity theme tokens
- `app/layout.tsx` — root layout with SEO metadata + viewport
- `app/page.tsx` — landing page composition
- `components/Hero.tsx` — hero section with value proposition
- `components/CTAButton.tsx` — primary/secondary CTA button
- `components/FeatureHighlights.tsx` — 3 feature cards
- `components/TrustSignals.tsx` — 3 speaker trust signals
- `components/AppStoreBadges.tsx` — App Store + Google Play links
- `components/Footer.tsx` — copyright footer
- `components/__tests__/Hero.test.tsx` — 3 tests
- `components/__tests__/FeatureHighlights.test.tsx` — 3 tests
- `components/__tests__/AppStoreBadges.test.tsx` — 3 tests
- `components/__tests__/CTAButton.test.tsx` — 3 tests
- `app/__tests__/page.test.tsx` — 2 tests

**Modified files:**
- `package.json` (root) — added `public`, `public:dev`, `public:build` scripts; updated `dev` to include public
- `.github/workflows/ci.yml` — added "Build public landing page" step
- `vitest.config.ts` (root) — added `test.projects` for workspace test discovery

## Change Log

| Date | Change | Author |
|------|--------|--------|
| 2026-02-12 | Story drafted from tech spec and epics | SM Agent |
| 2026-02-12 | Implementation complete — all tasks, tests, and ACs done | Dev Agent (Claude Opus 4.6) |
