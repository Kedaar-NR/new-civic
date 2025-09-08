# Civic Directory — elegant, trustworthy, and fast

A premium, calm civic directory for web (Next.js) and mobile (Expo), sharing design tokens and UI primitives in a single monorepo.

## Tech
- Web: Next.js (App Router), TypeScript, Tailwind, Radix/shadcn, Framer Motion
- Mobile: Expo (React Native), TypeScript, NativeWind, Reanimated, Moti
- Shared: packages for tokens, ui, types, data (adapters), utils
- Data: Google Civic Information API (baseline), OpenStates (state legs), official sources when permitted

## Monorepo layout
- apps/web
  - app/(components): ThemeToggle, Onboarding, Filters, LocationChip, MapPanel
  - app/page.tsx: search, results grid, right-side inspector
  - app/api/search/route.ts: server endpoint using data adapters
  - public/seed: offline demo JSON (94720, 10001)
- apps/mobile
  - App.tsx (providers), src/offline.ts (recents)
- packages
  - ui (shared primitives), tokens, types, data, utils
- scripts/deploy-azure-web.sh, Dockerfile.web

## Getting started
```bash
npm install
npm run dev
```
- Web: http://localhost:3000
- Mobile: cd apps/mobile && npm run ios|android|web

## Environment
Create .env at repo root:
```
GOOGLE_CIVIC_API_KEY=
OPENSTATES_API_KEY=
NEXT_PUBLIC_MAPBOX_TOKEN=
```
Notes:
- The web client calls /api/search, so secrets stay server-side.
- If keys are missing, UI runs on seed/mocked data.

## Seed data (offline demo)
```bash
npm run -w apps/web seed
```
Writes JSON to apps/web/public/seed for ZIPs 94720 and 10001.

## Testing
```bash
npm run test
```
Vitest is configured for units. Extend with RTL/Playwright as needed.

## Design tokens
Color, radius, motion, and shadow live in packages/tokens. UI respects light/dark themes and prefers-reduced-motion.

## Accessibility
- WCAG AA contrast, keyboard navigation, focus rings
- ARIA and live regions for async content and toasts

## Deploy (Docker + Azure App Service)
Local container:
```bash
docker build -f Dockerfile.web -t newcivic-web:latest .
docker run --rm -p 3000:3000 -e GOOGLE_CIVIC_API_KEY=your_key newcivic-web:latest
```
Azure App Service:
```bash
export GOOGLE_CIVIC_API_KEY=your_key
bash scripts/deploy-azure-web.sh
```

## Notes
- Verified fields show a chip with source and date. Embeds fall back to an official portal link if blocked by CSP.
- Map uses Mapbox when NEXT_PUBLIC_MAPBOX_TOKEN is present, otherwise a graceful placeholder is shown.

## License
MIT (or update as needed)
