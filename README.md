# Sales CRM

A lightweight CRM application for sales teams to manage contacts, track deals through a pipeline, and capture notes — with a manager dashboard for team performance visibility.

## Features

- **Contact Management** — Create, edit, and soft-delete contacts with search and pagination
- **Deal Pipeline** — Track deals through stages (New, Qualified, Won, Lost) with Kanban board view
- **Notes** — Attach notes to contacts and deals to preserve call/meeting context
- **Manager Dashboard** — Aggregate view of deal counts and pipeline value by stage

## Tech Stack

- React 19 + TypeScript
- Tailwind CSS v4 + shadcn/ui
- React Router v7
- TanStack Query
- React Hook Form + Zod
- Supabase (database, auth, RLS)
- react-i18next (FR, EN, DE, LU)

## Getting Started

```bash
npm install
npm run dev
```

Open http://localhost:5173

## Environment Variables

Copy `.env.example` to `.env` and fill in your Supabase credentials:

```
VITE_SUPABASE_URL=https://xxx.supabase.co
VITE_SUPABASE_ANON_KEY=eyJ...
```

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Production build |
| `npm run preview` | Preview production build |
| `npm run lint` | Run linter |
