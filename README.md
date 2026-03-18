# LAPSE Dashboard

An interactive research platform exploring Canadian federal and British Columbia provincial legislation relevant to Pacific salmon stewardship.

## Overview

The **LAPSE Dashboard** is a React-based application designed to help policymakers, researchers, and environmental professionals understand and analyze legislation affecting Pacific salmon. The app maps federal and provincial legislation to management domains and IUCN-CMP threat categories, enabling comprehensive policy research and cross-domain analysis.

### Key Features
- **Multi-dimensional filtering**: Filter legislation by jurisdiction (Federal/Provincial), management domain, act name, and keywords
- **Interactive visualizations**: Explore keyword frequency, IUCN-CMP threat distributions, and legislation coverage
- **Detailed legislation search**: Browse and search individual legislative clauses with full context
- **Domain mapping**: Understand how legislation connects to specific management domains and conservation threat categories
- **Responsive design**: Fully responsive interface for desktop and tablet viewing

## Technology Stack

- **Frontend Framework**: React 19
- **Language**: TypeScript (~5.8.2)
- **Build Tool**: Vite
- **Visualization**: Recharts (bar charts, pie charts)
- **Styling**: Tailwind CSS
- **Data Processing**: XLSX file parsing, JSON data sources

## Project Structure

```
LAPSE_dashboard/
├── public/                    # Static assets and data files
│   ├── LAPSE_compendium.json # Main legislation dataset
│   ├── governance_keywords.csv
│   ├── iucn_l2_keywords.csv
│   └── LAPSE-*.html          # Documentation files
├── src/
│   ├── components/           # React components
│   │   ├── App.tsx          # Main app orchestrator
│   │   ├── Filters.tsx      # Filter UI component
│   │   ├── ClauseFilters.tsx # Clause-specific filters
│   │   ├── Visualizations.tsx # Chart visualizations
│   │   ├── LegislationList.tsx # Searchable legislation view
│   │   └── Sidebar.tsx      # Navigation and modals
│   ├── services/
│   │   └── dataService.ts   # Data loading and processing
│   ├── constants.tsx        # Constants and color mappings
│   ├── types.ts             # TypeScript type definitions
│   ├── index.tsx            # React entry point
│   └── index.html           # HTML template
├── vite.config.ts           # Vite configuration
├── tsconfig.json            # TypeScript configuration
└── package.json             # Dependencies and scripts
```

## Understanding the Workflow

### Data Pipeline
1. **Load**: App fetches legislation data from `public/LAPSE_compendium.json`
2. **Filter**: Multi-dimensional filters (jurisdiction, domain, act, keywords) narrow the dataset
3. **Compute**: Visualizations and statistics aggregated in real-time from filtered data
4. **Display**: Results shown across three views: dashboard, visualizations, and detailed list

### Core Data Structure

Each legislation item contains:
- **Metadata**: Act name, legislation name, section, heading
- **Content**: Full aggregate paragraph text
- **Classification**: Management domain, IUCN threat category, clause type
- **Keywords**: Domain-specific keywords for filtering and visualization

```typescript
LegislationItem {
  id: string;
  jurisdiction: 'Federal' | 'Provincial';
  act_name: string;
  legislation_name: string;
  section: string;
  heading: string;
  aggregate_paragraph: string;
  management_domain: string;
  iucn_threat: string;
  clause_type: string;
  management_domain_keywords: string;
  clause_type_keywords: string;
  aggregate_keywords: string;
}
```

### Three Primary Tabs

1. **Dashboard**: High-level statistics and overview of loaded data
2. **Explorer**: Interactive filters, visualizations, and keyword analysis
3. **Legislation List**: Detailed, searchable view of all legislation items

## Getting Started

### Prerequisites
- Node.js 18+ 
- npm or yarn

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/dfo-pacific-science/LAPSE_dashboard.git
   cd LAPSE_dashboard
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Ensure `public/LAPSE_compendium.json` exists with your legislation dataset

### Development

Start the development server:
```bash
npm run dev
```

The app will be available at `http://localhost:5173` in your browser.

**Live reload enabled**: Changes to source files automatically refresh the app.

### Building for Production

Build the optimized production bundle:
```bash
npm run build
```

Output is written to the `dist/` directory.

### Preview Production Build

Test the production build locally:
```bash
npm run preview
```

## Deployment

### GitHub Pages Deployment

The app is configured for deployment to GitHub Pages with a base URL of `/LAPSE_dashboard/`. 

**Configuration**: See `vite.config.ts` line 9 for the base URL setting.

To deploy to a different repository or domain:
1. Update the `base` property in `vite.config.ts`
2. Ensure static assets in `public/` are accessible at the new path
3. Run `npm run build` and deploy the `dist/` directory

### Data File Loading

The app attempts to load `LAPSE_compendium.json` from three possible paths (in order):
1. `LAPSE_compendium.json`
2. `./LAPSE_compendium.json`
3. `/LAPSE_compendium.json`

Diagnostic logs display in the UI to help troubleshoot data loading issues.

## Key Architecture Patterns

### Filter Computation
Filters use controlled component pattern with `useCallback` handlers updating `FilterState`. Derived options (act counts, legislation counts) are computed with `useMemo` hooks. All dimensions filter against current state.

### Keyword Processing
Keywords are semicolon-separated strings. Always parse with:
```typescript
keywords.split(';').map(k => k.trim())
```

### Responsive Design
Mobile-first Tailwind CSS approach. Components use flex/grid with responsive breakpoints (e.g., `lg:grid-cols-2`).

### State Management
App maintains centralized state for:
- Loaded legislation data
- Filter selections (jurisdiction, domain, act, keywords)
- Active tab/view
- Diagnostic messages

## Development Guidelines

### Adding a New Filter
1. Extend `FilterState` interface in `types.ts`
2. Add filter UI to `Filters.tsx`
3. Compute unique filter options in `App.tsx` with `useMemo`
4. Apply filter in data filtering logic

### Adding Visualizations
1. Create new `useMemo` hook to aggregate data
2. Add Recharts component to `Visualizations.tsx`
3. Include custom Tooltip styling and consistent color palette
4. Pre-sort data by descending value

### Modifying Data Columns
1. Update `LegislationItem` interface in `types.ts`
2. Verify `public/LAPSE_compendium.json` contains matching fields
3. Update any dependent filters or visualizations

## Testing & Troubleshooting

- **Browser console**: Check for Vite build errors and JSON loading diagnostics
- **Diagnostic log**: Real-time diagnostics displayed in the app UI
- **Responsive testing**: Test across different viewport sizes with browser dev tools

## Additional Resources

- [LAPSE Technical Brief](./public/LAPSE-Technical-Brief.html)
- [Dashboard About/Help](./public/LAPSE-Dashboard-About.html)
- [Copilot Instructions](./copilot-instructions.md) - Development guidelines and patterns

## License

Please refer to the LICENSE file in the repository for terms of use.

## Support & Contribution

For issues, questions, or contributions, please open an issue or pull request in the repository.
