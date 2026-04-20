# Project Conventions

## Supabase

- **Project Ref** : `zaepkrrofxoeswgggbfi`
- **Dashboard** : https://supabase.com/dashboard/project/zaepkrrofxoeswgggbfi

### Migrations (MANDATORY)

**ALWAYS** create a migration file before modifying the database:

```
supabase/migrations/
└── YYYYMMDDHHMMSS_description.sql
```

Example: `20250102143000_create_users_table.sql`

**NEVER**:
- Modify the DB directly via the dashboard without a migration
- Use the SQL Editor for permanent changes
- Apply modifications without a versioned migration file

**Process**:
1. Create the migration file in `supabase/migrations/`
2. Write the SQL (CREATE, ALTER, etc.)
3. Apply via MCP or `supabase db push`
4. Commit the migration file

### Row Level Security (RLS)

- Enable RLS on ALL tables
- Create explicit policies for each operation (SELECT, INSERT, UPDATE, DELETE)
- Test policies with different roles

## Tech Stack

| Category | Technology |
|----------|------------|
| Build | Vite |
| Frontend | React 19 + TypeScript |
| Styling | Tailwind CSS v4 + shadcn/ui |
| Routing | React Router v7 |
| State | TanStack Query |
| Forms | React Hook Form + Zod |
| Backend | Supabase |
| i18n | react-i18next |

## Internationalization (i18n)

The application must support **4 languages**:

| Code | Language |
|------|----------|
| `fr` | Francais (default) |
| `en` | English |
| `de` | Deutsch |
| `lu` | Letzebuergesch |

### Translation structure

```
src/locales/
├── fr/
│   └── translation.json
├── en/
│   └── translation.json
├── de/
│   └── translation.json
└── lu/
    └── translation.json
```

### Rules

- **No hardcoded text** in components
- Use `useTranslation()` for all text
- Translation keys in English, dot notation format: `common.buttons.submit`
- Always add all 4 languages at the same time

## Responsive Design

The application must be **mobile-first** and work on:

| Breakpoint | Size | Usage |
|------------|------|-------|
| `sm` | 640px+ | Large mobile |
| `md` | 768px+ | Tablet |
| `lg` | 1024px+ | Desktop |
| `xl` | 1280px+ | Large desktop |

### Rules

- Start with mobile design
- Use Tailwind responsive classes (`sm:`, `md:`, etc.)
- Test on mobile, tablet and desktop
- No horizontal scrolling

## SEO

### Rules

- Each page must have a unique, descriptive `<title>`
- Use appropriate `<meta description>` tags
- Semantic HTML structure (`<header>`, `<main>`, `<nav>`, `<article>`, etc.)
- Images with descriptive `alt` attribute
- Clean, readable URLs
- Open Graph tags for social sharing

### SEO Component

Use an `<SEO>` component for each page:

```tsx
<SEO
  title="Page Title"
  description="Description for search engines"
/>
```

## Code Quality

### TypeScript

- **Strict mode** enabled
- No `any` except for documented exceptional cases
- Interfaces for component props
- Types for API responses

### Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase + use | `useAuth.ts` |
| Utilities | camelCase | `formatDate.ts` |
| Types | PascalCase | `UserType` |
| Constants | UPPER_SNAKE_CASE | `API_URL` |

### Imports

Always use `@/` aliases:

```ts
import { Button } from '@/components/ui/button'
import { useAuth } from '@/hooks/useAuth'
```

### Component Structure

```tsx
// 1. Imports
import { useState } from 'react'

// 2. Types
interface Props {
  title: string
}

// 3. Component
export function MyComponent({ title }: Props) {
  // 4. Hooks
  const [state, setState] = useState()

  // 5. Handlers
  const handleClick = () => {}

  // 6. Render
  return <div>{title}</div>
}
```

### Files

- One component per file
- File name = component name
- Index files for grouped exports

## Project Structure

```
src/
├── components/
│   ├── ui/              # shadcn/ui components (do not modify)
│   ├── layout/          # Header, Footer, Sidebar
│   └── features/        # Business components by feature
├── hooks/               # Custom hooks
├── lib/                 # Utilities
├── locales/             # Translation files
├── pages/               # Pages/routes
├── services/            # API calls, Supabase client
└── types/               # Global TypeScript types
```

## Commands

```bash
npm run dev      # Development server
npm run build    # Production build
npm run preview  # Build preview
npm run lint     # Linter
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `VITE_SUPABASE_URL` | Supabase project URL |
| `VITE_SUPABASE_ANON_KEY` | Supabase public key |

## Git

### Commits

Format: `type: description`

Types:
- `feat:` new feature
- `fix:` bug fix
- `refactor:` refactoring
- `style:` formatting, CSS
- `docs:` documentation
- `test:` tests
- `chore:` maintenance

### Branches

- `main` : production
- `develop` : development
- `feature/xxx` : new features
- `fix/xxx` : bug fixes
