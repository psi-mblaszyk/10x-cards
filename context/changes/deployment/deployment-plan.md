# Deployment Plan: Vercel Integration

## Objective

Transition the Astro project from Cloudflare Pages to Vercel, aligning with the architectural decisions in `context/foundation/infrastructure.md` and `context/foundation/tech-stack.md`. We will set up the deployment using the Vercel CLI to establish the project and enable GitOps (auto-deploy on push to `master`).

## Key Files & Context

- `astro.config.mjs` (Change adapter)
- `package.json` (Change dependencies)
- `wrangler.jsonc` (To be removed)
- `context/foundation/infrastructure.md` (Reference)

## Prerequisites (Human Actions)

Before starting the deployment process, ensure the following tools are installed and accounts are configured:

- **Node.js**: Ensure Node.js (v22 or later) and npm are installed locally.
- **GitHub Account**: A GitHub account is required to host the repository and enable GitOps auto-deployments.
- **Vercel Account**: Create a free Hobby account at [Vercel](https://vercel.com/signup). You will connect this to your GitHub account during the CLI setup.
- **Supabase Account**: Create a project in [Supabase](https://supabase.com/dashboard) and ensure your database is provisioned.

## Implementation Steps

### 1. Codebase Modifications (Automated)

- [x] **Uninstall Cloudflare**: Remove `@astrojs/cloudflare` and `wrangler` from `package.json`.
- [x] **Delete Cloudflare Configs**: Remove `wrangler.jsonc` and the `.wrangler` directory.
- [x] **Install Vercel Adapter**: Add `@astrojs/vercel` to `package.json` dependencies.
- [x] **Update Astro Config**: Modify `astro.config.mjs` to import and use the Vercel adapter (`output: "server"` will be maintained).
- [x] **Update Ignore Files**: Add `.vercel/` to `.gitignore`.

### 2. Manual Setup Gates (Human Actions)

Before the automated CI/CD can connect to your database, you must obtain and configure the correct connection secrets:

- [x] **Obtain Supabase API URL**:
  - Go to your Supabase Project Dashboard -> Project Settings -> API.
  - Copy the **Project URL** (under "Project API keys", it should look like `https://xxxxxx.supabase.co`).
  - **Note**: Do _not_ use the PostgreSQL connection string (`postgres://...`) for `SUPABASE_URL`, as the Supabase JS/Astro SDK communicates via HTTPS API endpoints.
- [x] **Obtain Supabase Anon Key**:
  - Go to Project Settings -> API.
  - Copy the `anon` `public` key.

### 3. CLI & Deployment (Automated & Interactive)

- [x] **Use Vercel CLI via npx**: (No global installation required).
- [x] **Login**: Run `npx vercel login` (interactive prompt for the user).
- [x] **Link & Deploy**: Run `npx vercel` to link the local project to a Vercel project and set up the GitHub repository connection for future auto-deployments on push to `master`.
- [x] **Set Secrets**: Add the `SUPABASE_URL` (using the transaction pooler string) and `SUPABASE_KEY` / `SUPABASE_ANON_KEY` secrets to the Vercel project. This can be done via the CLI (`npx vercel env add`) or via the Vercel web dashboard (Project Settings -> Environment Variables).
- [x] **Trigger Production Build**: Run `npx vercel --prod` to execute the first production deployment.

### 4. Edge Cases & Mitigations

- [ ] **Database Connections**: If `504 Gateway Timeout` errors occur on Vercel, double-check that the `SUPABASE_URL` environment variable uses the Transaction Pooler (port `6543`).
- [ ] **Vite Rollup Hash Bug**: If the Astro build fails with hash placeholder errors during deployment (common with complex React islands), we will pin the `astro` dependency to version `6.1.2` as documented in the infrastructure risks.

## Verification & Testing

- [x] Visit the generated Vercel production URL.
- [x] Ensure the Astro SSR pages load quickly (no 504 errors).
- [x] Confirm that pushing a test commit to the `master` branch automatically triggers a build in the Vercel dashboard.
- [x] Write this plan to `context/changes/deployment/deployment-plan.md` for milestone planning reference.
