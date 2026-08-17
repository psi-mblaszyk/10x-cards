# GEMINI.md - Instructional Context for AI Agent

This file serves as the definitive reference and context for any AI Agent working in this repository. It defines the project's technology stack, architecture, standard development workflows, and coding conventions.

---

## 1. Development Conventions

To maintain a cohesive and robust codebase, all agents must rigorously adhere to the following architectural guidelines and styling rules.

### Rendering Rules
- **Astro SSR Mode**: The application runs under "server" mode. All pages are server-rendered by default.
- **Serverless API Routes**: API endpoints must live in `src/pages/api/` and must export standard uppercase HTTP methods (e.g., `GET`, `POST`). They must export `const prerender = false`. Use `zod` to validate all incoming inputs.
- **Islands Architecture**: Use Astro components for static layouts and content. Transition to React components *only* when client-side interactivity is necessary.

### Coding Practices & Guidelines
- **Path Aliases**: Always use the `@/*` alias mapping to `./src/*` instead of relative paths (e.g., import from `@/lib/supabase`).
- **No Next.js Directives**: Do not write "use client" or "use server" directives in React files. These are not supported or needed by Astro's island architecture.
- **Tailwind Class Merging**: Always use the `cn()` utility from `@/lib/utils` (which wraps `clsx` and `tailwind-merge`) when conditionally merging classes or overriding components. Avoid manual concatenation of Tailwind utility classes.
- **UI Components**: Build or add new design primitives under `src/components/ui/` using shadcn/ui. Install via `npx shadcn@latest add [name]` following the `new-york` style variant.
- **Extracting Hooks & Logic**:
  - React hooks should be isolated and saved in `src/components/hooks/`.
  - Service modules, API integrators, and helper logic belong in `src/lib/` or `src/lib/services/`.
  - Shared typescript entities and data transfer object (DTO) models should be saved in `src/types.ts`.

### Database Migrations
- Write new schema modifications as SQL migration files under `supabase/migrations/` using the exact file name convention: `YYYYMMDDHHmmss_short_description.sql`.
- **Row Level Security (RLS)**: RLS must be explicitly enabled on all newly created tables. Every table must define granular, secure policies mapping actions to specific roles (e.g., authenticated users).

### Code Quality & Validation
- **CI Safety**: A GitHub Actions workflow (`.github/workflows/ci.yml`) runs verification tests (`npm run lint` and `npm run build`) on every push or pull request to the `master` branch.
- **Pre-commit Hooks**: Automated git hooks (Husky + lint-staged) execute before every commit:
  - Runs `eslint --fix` on `*.ts`, `*.tsx`, and `*.astro` files.
  - Runs `prettier --write` on `*.json`, `*.css`, and `*.md` files.

---

## 2. Project Overview

### Purpose
The **10x Astro Starter** is a modern, opinionated starter template designed for building highly performant, server-first, type-safe web applications deployed on edge runtimes. It is built to optimize developer productivity and deliver polished, fast, and accessible user experiences.

### Key Technologies
- **Framework**: [Astro v6](https://astro.build/) in Server-Side Rendering (SSR) mode (`output: "server"`).
- **Frontend Islands**: [React v19](https://react.dev/) (used selectively for interactive islands).
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) (integrated via `@tailwindcss/vite`).
- **Type Safety**: [TypeScript v5](https://www.typescriptlang.org/) (strict mode configured via `astro/tsconfigs/strict`).
- **Backend & Authentication**: [Supabase](https://supabase.com/) (using `@supabase/ssr` for cookie-based session management).
- **Deployment & Runtime**: [Cloudflare Workers](https://workers.cloudflare.com/) (configured via `wrangler.jsonc` with the `@astrojs/cloudflare` adapter).

### Architecture & Folder Structure
*(See also `@README.md` for layout mappings)*
```md
.
├── .github/workflows/  # CI pipelines (lint and build on push/PR to master)
├── .husky/             # Git pre-commit hooks
├── public/             # Static public assets
├── src/
│   ├── components/     # UI Components
│   │   ├── auth/       # Authentication-related components (SignIn, SignUp, etc.)
│   │   └── ui/         # Reusable shadcn/ui base components
│   ├── layouts/        # Layout wrappers (e.g., Layout.astro)
│   ├── lib/            # Shared libraries, helpers, and client initializers
│   │   ├── supabase.ts # Supabase SSR client creator
│   │   └── utils.ts    # Utility functions (including the cn Tailwind merger)
│   ├── pages/          # Routing layer (Astro file-based routing)
│   │   ├── api/        # Serverless API routes (signin, signup, signout)
│   │   ├── auth/       # Auth views (signin, signup, confirm-email)
│   │   └── dashboard.astro # Protected content dashboard
│   ├── styles/         # Global stylesheets (Tailwind imports)
│   └── middleware.ts   # Edge middleware checking session state per request
├── supabase/           # Local Supabase configurations & SQL migrations
├── astro.config.mjs    # Astro configuration (integrations, adapter, schema)
├── tsconfig.json       # TypeScript options and path mappings
└── wrangler.jsonc      # Cloudflare wrangler configuration
```

---

## 3. Building and Running

Ensure you have **Node.js v22.14.0** installed. Local database operations require **Docker** with at least ~7 GB of RAM allocated.

### Core CLI Scripts
*(See also `@package.json` and `@CLAUDE.md` for direct script configurations)*

| Command | Action |
|:---|:---|
| `npm run dev` | Starts the Astro development server in local Cloudflare `workerd` runtime. |
| `npm run build` | Compiles the production build (SSR compiled for `@astrojs/cloudflare`). |
| `npm run preview` | Previews the built production assets locally. |
| `npm run lint` | Runs ESLint across the codebase using type-checked rules. |
| `npm run lint:fix` | Runs ESLint and auto-fixes any repairable style/lint violations. |
| `npm run format` | Runs Prettier to enforce consistent formatting across all supported file types. |

### Database & Environment Setup
1. **Credentials Setup**:
   - Local Node/Supabase: copy `.env.example` to `.env`.
   - Cloudflare Local Dev: copy `.env.example` to `.dev.vars` (this is gitignored and loaded by wrangler).
2. **Local Supabase Start**:
   - Initialize Supabase project (if not already initialized): `npx supabase init`
   - Spin up Docker-based Supabase stack: `npx supabase start`
   - Copy the printed endpoint/key credentials directly into both `.env` and `.dev.vars`.
