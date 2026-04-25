---
name: LAPSE Dashboard workspace instructions
description: "Guidelines for AI contributions to the LAPSE Dashboard React + TypeScript application."
---

## Workspace Guidance
Use this file to guide AI contributions across the project. Focus on the existing React + TypeScript architecture, the JSON-driven data pipeline, Tailwind styling, and the app's filter/visualization patterns.

## Key Project Facts
- Single-page React app built with Vite and TypeScript.
- Main dataset is loaded from `public/LAPSE_compendium.json`.
- UI is composed of dashboard, explorer, and legislation list views.
- Visualizations use Recharts and derived data is computed with `useMemo`.
- Filters are controlled from `App.tsx` and passed into child components.

## Important Files
- `App.tsx` — state orchestration, data loading, filters, tab routing.
- `components/Filters.tsx` — filter UI for jurisdiction, management domain, act, legislation, and search.
- `components/Visualizations.tsx` — chart renderers and data aggregation logic.
- `components/LegislationList.tsx` — searchable list of filtered legislation.
- `components/Sidebar.tsx` — navigation, help modal, and tab controls.
- `types.ts` — TypeScript interfaces for legislation items and filter state.
- `constants.tsx` — mappings for jurisdictions, domains, and colors.
- `vite.config.ts` — base path config for GitHub Pages deployment.

## Build & Dev Commands
- `npm install`
- `npm run dev`
- `npm run build`
- `npm run preview`

## Conventions
- Use strict TypeScript typing and React function components.
- Keep Tailwind utility-first styling consistent with existing layout patterns.
- Parse keyword fields by splitting semicolon-delimited text and trimming each entry.
- When adding new filters, update both the UI component and filter logic in `App.tsx`.
- When adding charts, compute aggregated data in `useMemo`, sort descending, and use responsive Recharts containers.

## Data & Loading Gotchas
- The app attempts three fetch paths for the main JSON data: `LAPSE_compendium.json`, `./LAPSE_compendium.json`, and `/LAPSE_compendium.json`.
- Keep the dataset in `public/LAPSE_compendium.json` aligned with `types.ts` field names.

## Style Notes
- Responsive layout is mobile-first with Tailwind breakpoints such as `lg:grid-cols-2`.
- Charts and lists should preserve existing spacing, typography, and color usage.
- Prefer existing component patterns over introducing unrelated abstractions.

## What to Avoid
- Do not change the deployment `base` path in `vite.config.ts` unless updating GitHub Pages configuration.
- Do not create new global CSS files; stay within Tailwind utility classes.
- Do not bypass the app's controlled filter state model by adding ad hoc state in individual child components.

## Useful Context
Use the `README.md` for broader project context and the existing components as the source of truth for patterns and conventions.
