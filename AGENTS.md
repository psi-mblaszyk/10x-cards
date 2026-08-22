# Repository Guidelines

This repository contains the **10x Astro Starter**, a modern server-side rendered web application built with Astro v6, React v19, Tailwind CSS v4, and Supabase. Deployments target Cloudflare Workers.

## 1. Critical Rules & Constraints

*   **No Next.js Directives**: Never write `"use client"` or `"use server"` in React components. Astro manages interactivity islands natively.
*   **Tailwind Class Merging**: Never concatenate utility classes manually. Always use the `cn()` helper from `@/lib/utils.ts`.
*   **Row-Level Security (RLS)**: Every new database table must have RLS enabled with granular policies in a migration named `YYYYMMDDHHmmss_short_description.sql` under `supabase/migrations/`.
*   **Type Safety**: Do not bypass the type system or use `any` type casts. Resolve all TypeScript errors.
*   **Validation**: Every API route in `src/pages/api/` must export `const prerender = false` and validate all inputs with Zod.

## 2. Development & Build Commands

*   `npm run dev` – Start the local development server in the Cloudflare workerd runtime.
*   `npm run build` – Compile the production build optimized for `@astrojs/cloudflare`.
*   `npm run lint` – Run ESLint type-checked rules (defined in `@eslint.config.js`).
*   `npm run format` – Format files using Prettier (defined in `@.prettierrc.json`).

Pre-commit hooks (Husky + lint-staged) automatically run `eslint --fix` on `*.{ts,tsx,astro}` and `prettier --write` on `*.{json,css,md}` before every commit.

## 3. Project Structure & Conventions

*   **Path Aliases**: Always use `@/*` to reference `src/*` paths (configured in `@tsconfig.json`).
*   **Islands Architecture**: Use Astro components for layout and static content. Limit React components to interactive clients.
*   **File Locations**:
    *   React hooks: `src/components/hooks/`
    *   Business logic & services: `src/lib/` or `src/lib/services/`
    *   Shared types/DTOs: `src/types.ts`
    *   UI primitives: `src/components/ui/` (install via `npx shadcn@latest add [name]`)

## 4. Git & CI Workflows

*   **CI Pipeline**: GitHub Actions (`@.github/workflows/ci.yml`) runs `lint` and `build` checks on pushes and PRs to `master`.
*   **Commits**: Use short, lowercase, imperative commit messages (e.g. `select tech stack`, `prd updated`) aligned with the repository's git history.
