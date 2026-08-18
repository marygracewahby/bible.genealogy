# The Biblical Family Tree

An interactive visual genealogy of people named in the Bible, allowing users to follow parent-to-child relationships through connected family-tree cards.

## Features

- **Interactive Family Tree**: Expandable genealogy from Adam to Jesus Christ
- **Comprehensive Data**: Every person named in the biblical genealogy with unique IDs
- **Story Tooltips**: Hover/focus on people with asterisks to view story summaries and relevant Bible passages
- **Kings & Judges Reference**: Separate sections for biblical kings and judges with timelines
- **Search Functionality**: Highlight matching person cards
- **Fully Responsive**: Desktop, tablet, and mobile support
- **Accessible**: Semantic HTML, ARIA labels, keyboard navigation, reduced-motion support
- **Type-Safe**: Built with TypeScript for data integrity
- **Data Validation**: Detects duplicate IDs, broken links, and missing references

## Project Structure

```
biblical-family-tree/
├── src/
│   ├── data/
│   │   ├── genealogy.ts          # Main family tree data
│   │   ├── kings.ts              # Kings reference data
│   │   ├── judges.ts             # Judges reference data
│   │   ├── stories.ts            # Story content for people
│   │   └── types.ts              # Type definitions
│   ├── components/
│   │   ├── FamilyTree/           # Tree visualization
│   │   ├── PersonCard/           # Individual person cards
│   │   ├── StoryTooltip/         # Story popover
│   │   ├── KingsSection/         # Kings reference section
│   │   ├── JudgesSection/        # Judges reference section
│   │   ├── Search/               # Search functionality
│   │   ├── Timeline/             # Era timeline components
│   │   ├── Legend/               # Visual legend
│   │   └── Layout/               # Page layout
│   ├── lib/
│   │   ├── validation.ts         # Data validation utilities
│   │   ├── search.ts             # Search logic
│   │   └── utils.ts              # Helper functions
│   ├── styles/
│   │   ├── globals.css           # Global styles
│   │   ├── genealogy.css         # Genealogy tree styles
│   │   ├── cards.css             # Card component styles
│   │   ├── responsive.css        # Responsive design
│   │   └── accessibility.css     # Accessibility enhancements
│   └── pages/
│       ├── _app.tsx              # Next.js app wrapper
│       ├── _document.tsx         # HTML document template
│       ├── index.tsx             # Home/genealogy page
│       ├── kings.tsx             # Kings reference page
│       └── judges.tsx            # Judges reference page
├── scripts/
│   └── validate-data.js          # Data validation CLI
├── public/
│   └── favicon.ico
├── jest.config.js                # Jest testing config
├── next.config.js
├── tsconfig.json
├── .eslintrc.json
├── .gitignore
├── package.json
└── README.md
```

## Setup & Development

### Prerequisites
- Node.js 18+
- npm or yarn

### Installation

```bash
# Clone the repository
git clone https://github.com/marygracewahby/bible.genealogy.git
cd bible.genealogy

# Install dependencies
npm install
```

### Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Commands

```bash
# Type checking
npm run type-check

# Linting
npm run lint
npm run lint:fix

# Data validation
npm run validate

# Run all checks and build
npm run build

# Production server
npm run start

# Testing
npm run test
npm run test:watch
```

## Data Architecture

### Unique IDs
Every person has a unique, stable ID based on their scriptural identity:
- Format: `{descriptor}-{distinguisher}` (e.g., `adam`, `cain`, `seth`)
- Never resolved by name alone (many biblical figures share names)
- Cross-referenced in relationships, stories, kings, judges, and spouse data

### Data Files

#### genealogy.ts
Structured family tree data with:
- `people`: Array of all individuals with ID, name, parents, spouses, children
- `relationships`: Direct parent-child and spousal connections
- No duplicate IDs
- All parent/spouse references validated

#### stories.ts
Story content indexed by person ID:
- Summary text
- Relevant Bible passages
- Only for people with actual scriptural narratives

#### kings.ts
Kings and rulers indexed by person ID:
- Kingdom or realm
- Genealogical reference
- Reign passages
- Overview
- Biblical evaluation
- Connection to genealogy (if applicable)

#### judges.ts
Judges from Othniel through Samuel:
- ID references
- Tenure overview
- Verse references
- Timeline context

## Validation

Run validation to check for:
- Duplicate person IDs
- Missing parent/spouse references
- Broken story links
- Asterisks without stories
- Invalid king/judge links
- Unreferenced people

```bash
npm run validate
```

## Deployment

### Vercel (Recommended)

```bash
npm run build
npm run start
```

Or connect your GitHub repo to Vercel for automatic deployments.

### Docker

```dockerfile
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run validate
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV production
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
EXPOSE 3000
CMD ["node", "server.js"]
```

### Static Export

```bash
NODE_ENV=production npm run build
# Output in ./out directory
```

## Accessibility

- Semantic HTML5 elements
- ARIA labels on interactive elements
- Keyboard navigation (Tab, Enter, Escape, Arrow keys)
- Focus management for tooltips
- `prefers-reduced-motion` support
- Color contrast compliance (WCAG AA minimum)
- Screen reader friendly tree structure

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile browsers (iOS Safari, Chrome Mobile)

## License

MIT
