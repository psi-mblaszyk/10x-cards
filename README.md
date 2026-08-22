# 10xCards 📇

> **AI-Powered Active Recall & Spaced Repetition Flashcards**

10xCards is a modern, high-performance web application designed to eliminate the manual friction of flashcard creation. Rather than wasting hours copying, pasting, and formatting questions, learners can instantly transform dense study guides, lecture notes, or textbook chapters into atomic, study-ready flashcards. Powered by an intelligent AI extraction pipeline and backed by an adaptive Spaced Repetition scheduling engine, 10xCards helps students and professionals achieve optimal knowledge retention with minimal effort.

---

## 🌟 Vision & Key Features

Modern flashcard tools focus heavily on deck management, but ignore the massive time sink required to create decks. **10xCards** solves the creation bottleneck for Alex, the Exam Prep Learner, and other active self-directed study personas by providing a seamless, fast AI extraction pipeline.

### Core Capabilities

- **🤖 AI-Powered Card Extraction (US-01 / FR-008 & FR-009)**: Paste raw notes (50 to 10,000 characters) and instantly extract 3 to 10 high-quality flashcards. Before saving, preview drafts, select/deselect specific cards, and edit the Question (Front) and Answer (Back) to refine study material.
- **🔑 Secure Cookie-Based Authentication (US-02 / FR-001 & FR-002)**: Complete, secure multi-user environment powered by **Supabase Auth** (`@supabase/ssr`). Gated routes (such as dashboards and active reviews) are strictly protected at the edge via Astro Middleware.
- **📂 Deck & Card Management (US-03 & US-04)**: Create custom named decks, view a clear dashboard summarizing total cards and active card review queues, or manually create, edit, and delete cards as needed.
- **⏱️ Spaced Repetition Study Engine (US-05 / FR-010 & FR-011)**: Review cards due based on an adaptive scheduling algorithm modeled on SuperMemo-2 (SM-2). Rate recall as **Easy**, **Good**, or **Hard** to automatically adjust review intervals. Transitions are designed to render instantly (< 100ms) to maintain deep focus.
- **📤 Anki-Compatible Export (FR-012)**: Export flashcards in CSV/TXT format to easily fit into existing power-user workflows.

---

## 🛠️ Tech Stack & Architecture

Built with a modern, server-first, type-safe stack optimized for speed, reliability, and serverless hosting compatibility:

| Component               | Technology                                       | Description                                                                                                            |
| :---------------------- | :----------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------- |
| **Framework**           | [Astro v6](https://astro.build/)                 | Configured in **Server-Side Rendering (SSR) mode** (`output: "server"`) for edge-native, fast dynamic delivery.        |
| **Interactive Islands** | [React v19](https://react.dev/)                  | Employed selectively for rich client-side interactivity (e.g., auth forms, flashcard review screens, draft selectors). |
| **Styling**             | [Tailwind CSS v4](https://tailwindcss.com/)      | Integrated via `@tailwindcss/vite` and paired with **shadcn/ui** (New York style) for cohesive, accessible aesthetics. |
| **Type Safety**         | [TypeScript v5](https://www.typescriptlang.org/) | Enforces strict type boundaries configured via `astro/tsconfigs/strict`.                                               |
| **Backend & Auth**      | [Supabase SSR](https://supabase.com/)            | Managed cloud database and authentication using secure, cookie-based sessions.                                         |
| **Deployment Runtime**  | [Vercel](https://vercel.com/)                    | Hosted on Vercel's serverless and edge-native runtime utilizing the `@astrojs/vercel` adapter.                         |

---

## 📁 Project Directory Structure

Following Astro's Islands Architecture and 10xDevs conventions:

```md
.
├── .github/workflows/ # CI workflows (Lint and build validation on push/PR)
├── .husky/ # Husky git hooks (Pre-commit linting and formatting)
├── public/ # Static assets (favicons, templates)
├── src/
│ ├── components/ # UI Components (Astro & React)
│ │ ├── auth/ # React Interactive Auth components (SignInForm, SignUpForm, etc.)
│ │ └── ui/ # Reusable atomic design primitives (buttons, inputs via shadcn/ui)
│ ├── layouts/ # Global page layout templates (Layout.astro)
│ ├── lib/ # Shared integrations and helper services
│ │ ├── supabase.ts # Supabase client factory for SSR cookie-based sessions
│ │ └── utils.ts # Utility functions (including the class merger cn() helper)
│ ├── pages/ # Astro file-based routing layer
│ │ ├── api/ # Serverless API routes (signin.ts, signup.ts, signout.ts)
│ │ ├── auth/ # User auth views (signin, signup, confirm-email)
│ │ ├── dashboard.astro # Main dashboard for authenticated users
│ │ └── index.astro # Public marketing landing page
│ ├── styles/ # Global CSS stylesheets (Tailwind v4 directives)
│ └── middleware.ts # Edge middleware controlling session checks and gated route redirects
├── supabase/ # Local Supabase configurations & SQL migrations
│ ├── config.toml # Local dockerized Supabase configuration
│ └── migrations/ # Versioned, reproducible SQL schema migrations
├── astro.config.mjs # Astro integration configuration and adapters (configured with Vercel)
└── tsconfig.json # TypeScript configuration and path aliases
```

---

## 🚀 Getting Started

Follow these steps to run the complete local environment, including a dockerized Supabase backend.

### Prerequisites

- **Node.js**: `v22.14.0` (as defined in `.nvmrc`)
- **Docker Desktop**: Required to run the local Supabase emulator stack (~7 GB RAM recommended).

### 1. Installation

Clone the repository and install dependencies:

```bash
git clone <your-repository-url>
cd 10x-cards
npm install
```

### 2. Configure Environment Variables

Copy the template configuration file to create your local environment secrets store:

```bash
cp .env.example .env
```

### 3. Spin up Local Supabase Stack

Initialize the local Supabase container environment and start the Docker services:

```bash
npx supabase init
npx supabase start
```

_Note: The first run will download the necessary Docker images, which may take a few minutes._

### 4. Wire up Credentials

Once started, the Supabase CLI will output your local API keys and URLs. Copy these credentials directly into `.env`:

```env
SUPABASE_URL=http://127.0.0.1:54321
SUPABASE_KEY=<anon_key_from_cli_output>
```

_Note: You can access the local database admin studio dashboard at `http://localhost:54323`._

### 5. Disable Local Email Confirmation (Optional)

To skip confirming email links in local development:

1. Open your local Supabase Studio (`http://localhost:54323`).
2. Go to **Project Settings** (gear icon) ➔ **Auth**.
3. Toggle off **Enable Email Confirmation** under **User Signups**.

### 6. Run the Application

Start the local Astro development server:

```bash
npm run dev
```

Open [http://localhost:4321](http://localhost:4321) in your browser to explore the landing page.

Alternatively, to run in Vercel's edge-replicated dev environment locally:

```bash
# Install Vercel CLI globally if not already installed
npm install -g vercel

# Run the local development server linked with Vercel secrets
vercel dev
```

---

## 💾 Database Migrations & Security

Database updates must be versioned, secure, and fully auditable:

1.  **Versioned Migrations**: Create schema modifications as versioned SQL scripts under `supabase/migrations/` using the standard layout naming convention:
    ```bash
    supabase/migrations/YYYYMMDDHHmmss_short_description.sql
    ```
2.  **Row Level Security (RLS)**: RLS is a hard security constraint. Every table (e.g., `decks`, `cards`) must have RLS explicitly enabled, and define strict policies restricting CRUD operations to authenticated owners:

    ```sql
    alter table public.decks enable row level security;

    create policy "Users can only manage their own decks"
      on public.decks
      for all
      to authenticated
      using (auth.uid() = user_id)
      with check (auth.uid() = user_id);
    ```

---

## 🏗️ Development & Quality Standards

All changes submitted to this repository must respect the following standards to ensure production safety and consistency:

- **Path Aliases**: Always use the `@/*` absolute paths targeting `./src/*` (e.g., `import { supabase } from '@/lib/supabase'`). Avoid relative imports.
- **No Next.js Directives**: Refrain from using `"use client"` or `"use server"`. Astro manages islands explicitly through component hydration tags (e.g., `<SignInForm client:load />`).
- **Tailwind Class Merging**: Always use the `cn(...)` utility from `@/lib/utils` when combining conditional classes or overriding component properties to prevent CSS collisions.
- **Automated Quality Gates**:
  - **CI Safety**: Linting, type-checking, and build checks are run automatically on every pull request to `master` (via `.github/workflows/ci.yml`).
  - **Pre-commit Hooks**: Automatic git hooks (Husky + lint-staged) automatically format files (`prettier --write`) and resolve fixable linting issues (`eslint --fix`) on staged files prior to commits.
- **AI Domain Integrity**: The card generator must remain 100% grounded. It should extract and formulate facts _only_ directly present in the pasted source materials—it must never hallucinate external information.

---

## 🛠️ Available Scripts

Execute scripts via `npm run <script-name>`:

- `dev`: Starts the local Astro development server.
- `build`: Builds the static assets and compiles serverless functions for edge deployment.
- `preview`: Run a local preview of the compiled production build.
- `lint`: Analyze code health and search for style/type discrepancies.
- `lint:fix`: Automatically repair fixable linter discrepancies.
- `format`: Format all `.json`, `.css`, and `.md` files according to Prettier configuration.

---

## 🌐 Production Deployment

The project is configured for automated deployments to **Vercel** via Git Integration:

1.  **CI/CD Auto-Deploy**: Connecting your GitHub repository to Vercel automatically deploys every push or merge to the `master` branch.
2.  **Environment Variables**: Link your local environment secrets to the live deployment by setting the `SUPABASE_URL` and `SUPABASE_KEY` variables in the **Vercel Dashboard ➔ Project Settings ➔ Environment Variables** tab.
3.  **Manual Deployment**: Alternatively, you can deploy the build manually from your CLI:

    ```bash
    # Login to Vercel
    vercel login

    # Initialize and deploy
    vercel --prod
    ```

---

## 📄 License

This project is licensed under the MIT License.
