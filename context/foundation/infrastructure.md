---
project: "10xCards"
researched_at: 2026-08-22T00:00:00Z
recommended_platform: Vercel
runner_up: Cloudflare Pages
context_type: mvp
tech_stack:
  language: TypeScript
  framework: Astro v6
  runtime: Node.js (Vercel Serverless)
---

## Recommendation

**Deploy on Vercel.**

Following the developer interview and anti-bias review, Vercel is selected as the optimal MVP deployment platform. It delivers unparalleled developer experience, a first-class Model Context Protocol (MCP) server for automated agent maintenance, and a robust serverless Node.js execution environment that integrates flawlessly with Astro v6 SSR. While Cloudflare was the initial default, Vercel was preferred to avoid strict runtime isolate limitations and provide maximum library compatibility for downstream feature growth (such as parsing libraries), accepting Vercel's Hobby plan caps and Supabase connection pool risks into our mitigation roadmap.

## Platform Comparison

The evaluated platforms were scored against the five agent-friendly criteria. Below is the comparative matrix:

| Platform       | CLI-First | Managed / Serverless | Agent-Readable Docs | Stable Deploy API | MCP / AI Integration | Total Heuristic Score |
| :------------- | :-------: | :------------------: | :-----------------: | :---------------: | :------------------: | :-------------------: |
| **Vercel**     |   Pass    |         Pass         |        Pass         |       Pass        |         Pass         |       **5 / 5**       |
| **Cloudflare** |   Pass    |         Pass         |        Pass         |       Pass        |         Pass         |       **5 / 5**       |
| **Railway**    |   Pass    |         Pass         |        Pass         |       Pass        |         Pass         |       **5 / 5**       |
| **Render**     |   Pass    |         Pass         |        Pass         |       Pass        |         Pass         |       **5 / 5**       |
| **Netlify**    |  Partial  |         Pass         |        Pass         |       Pass        |         Pass         |      **4.5 / 5**      |
| **Fly.io**     |   Pass    |       Partial        |        Pass         |       Pass        |         Pass         |      **4.5 / 5**      |

### Evaluated Platforms Notes

- **Vercel**: Fully scriptable CLI, excellent Markdown documentation endpoints, and the official Vercel MCP server. Provides standard Node.js serverless execution, making library compatibility highly resilient.
- **Cloudflare Workers + Pages**: Extremely performant edge isolate runtime with a $0 free tier. However, edge isolate runtimes require strict code patterns (e.g. absolute URL fetches inside `workerd` prerendering) and lack native Node streams support, raising high compatibility risks.
- **Railway**: A robust container PaaS that provides zero-downtime deploys and managed databases. However, it lacks a sustainable free tier ($5/mo base + usage, totaling ~$8/mo for 100k requests) and requires swapping Astro config to `@astrojs/node`.
- **Render**: Excellent developer tooling and native markdown docs. However, its free tier enforces a 15-minute inactivity spin-down, introducing a severe ~1-minute cold start delay that violates NFR-001 (study transitions <100ms).
- **Netlify**: Great JAMstack hosting but lacks a CLI-first rollback command (must be triggered via web UI or REST API), introducing a partial gap in automated agent operations.
- **Fly.io**: Firecracker microVMs are powerful but introduce higher configuration surfaces (Dockerfiles, host bindings, port settings), which raises the risk of agent deployment misconfiguration.

### Shortlisted Platforms

#### 1. Vercel (Recommended)

- **Why it won**: Vercel offers an exceptionally polished, zero-config deployment workflow for Astro SSR, backed by standard Node.js serverless execution. Its official MCP server enables autonomous AI agents to query logs, inspect builds, and manage environment variables directly.

#### 2. Cloudflare Pages

- **Why it scored second**: Cloudflare offers unmatched global performance and a generous free tier ($0/mo up to 100k requests/day). However, runtime isolate limitations (e.g., local `workerd` prerender issues and edge compatibility boundaries) create friction compared to Vercel's standard Node runtime.

#### 3. Railway

- **Why it scored third**: Railway excels at running fully integrated container environments. The gap vs the recommendation is cost, as Railway lacks a free tier, making Vercel's $0 Hobby tier much more attractive for a solo developer launching an MVP.

## Anti-Bias Cross-Check: Vercel

To ensure our recommendation is grounded in reality, Vercel was evaluated against three adversarial lenses:

### Devil's Advocate — Weaknesses

1. **Strict Hobby Plan Limits**: Hobby accounts are strictly capped at 100 GB bandwidth and 100 build minutes per month. Since there are _no overage options_ on the Hobby tier, exceeding these limits results in immediate, automated suspension of the site until the next billing cycle.
2. **Cold Starts on Serverless Functions**: Ephemeral serverless function compute execution incurs a cold start of 200ms–800ms when idle, which conflicts with NFR-001's constraint of instant transitions (<100ms) for dynamic study screens if they rely on server-side computation.
3. **Database Neon Integration Lock-in**: Vercel recently sunsetted `@vercel/postgres` and `@vercel/kv` in favor of external marketplaces (Neon and Upstash). Direct marketplace integrations simplify provisioning but create vendor coupling, requiring `@neondatabase/serverless` instead of standard database drivers.
4. **Vite/Rollup Hash Placeholder Build Bug**: Heavy client-side React 19 packages or animations (like Framer Motion) built with Astro v6 can fail to build on Vercel's bundler due to Rollup's hash placeholder generation failures.

### Pre-Mortem — How This Could Fail

> Six months later, 10xCards became a huge hit among university students. However, the Hobby Vercel plan quickly turned into an operational nightmare. A crawler indexed all the dynamic deck endpoints, exhausting the 100 GB monthly bandwidth quota within 48 hours. Because there is no billing threshold safety valve on Hobby, Vercel instantly took the app offline, leaving Alex's users stranded during exam week. Upgrading to the Pro tier cost $20/seat/month, but the team's serverless function costs exploded due to unoptimized database connection handshakes. Without native edge poolers like Hyperdrive, each stateless cold start handshaked a new TCP session to Supabase, exhausting the database's max connection pool and causing frequent `504 Gateway Timeout` errors. The app was offline or slow when users needed it most.

### Unknown Unknowns

- **Vercel WebSockets Hobby Timeout Capped**: The newly launched WebSockets on Fluid Compute (in Public Beta as of February 2027) are forcibly terminated on Hobby tiers at the standard function timeout (typically 10s–60s). This makes active-connection features unviable unless upgraded to Pro.
- **Asset Cache Purging Latency**: Vercel Edge Network utilizes static asset caching, which can result in users seeing stale UI versions after minor deployments unless full client-side cache-busting URLs or specific Vercel-revalidation headers are configured.
- **Routing Limitations**: Vercel configuration (`vercel.json`) rewrites and redirects only apply to static pages. Any dynamic edge routing logic or custom auth header injection must be handled in Astro's local `middleware.ts`.

## Operational Story

How the chosen platform operates day to day:

- **Preview deploys**: Vercel automatically deploys every pull request and non-production branch push to a unique, shareable preview URL. Forked repository PRs from external contributors do not build by default for security.
- **Secrets**: Encrypted environment variables live in the Vercel Project Settings Vault. They are injected at runtime; developers cannot read decrypted secrets in the UI once set, and they must be updated via the CLI or Vercel dashboard.
- **Rollback**: Production deployments can be rolled back within seconds using `vercel rollback <deployment-id>` in the CLI or clicking "Rollback" in the Vercel dashboard. Data layer (Supabase migrations) must be manually migrated backwards, as Vercel does not roll back the external database.
- **Approval**: Publishing to production (merging to `main`/`master` or running `vercel --prod`), rotating the Supabase primary secrets, or deleting the project requires a manual human action. The AI agent may deploy preview branches and trigger non-production tests unattended.
- **Logs**: The agent can retrieve read-only build and runtime execution logs using the Vercel CLI command `vercel logs <deployment-url> --environment production` or through the Vercel MCP Server.

## Risk Register

| Risk                                    | Source           | Likelihood | Impact | Mitigation                                                                                                                                         |
| --------------------------------------- | ---------------- | ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Hobby Plan Hard Limit Block**         | Pre-mortem       | Medium     | High   | Set up billing alerts or trigger early migration to the Pro tier if monthly bandwidth approaches 80 GB.                                            |
| **Database Connection Pool Exhaustion** | Pre-mortem       | High       | High   | Utilize Supabase's transaction pooler (port 5432 or 6543) instead of direct session connections to avoid connection limits under serverless scale. |
| **Vite/Rollup Hash Placeholder Bug**    | Devil's advocate | Low        | High   | Pin the Astro version to `6.1.2` or load heavy client-side packages statically from `/public` if the Rollup packager throws hash errors.           |
| **WebSockets Timeout Cap**              | Unknown unknowns | Low        | Medium | Restrict interactive real-time polling to standard stateless request-response triggers and keep any rich real-time workflows on separate channels. |

## Getting Started

1. Install the Vercel CLI globally:
   ```bash
   npm install -g vercel
   ```
2. Install the Astro Vercel Adapter in the project:
   ```bash
   npx astro add vercel
   ```
   _This command will automatically install `@astrojs/vercel`, add the import and adapter to `astro.config.mjs`, and update `package.json`._
3. Connect the repository and initialize the project:
   ```bash
   vercel login
   vercel
   ```
4. Configure environment variables (like `SUPABASE_URL` and `SUPABASE_ANON_KEY`) in the Vercel Project dashboard.
5. Deploy to production by pushing to the default branch (integrated via GitHub Actions or Vercel's Git Integration) or manually via:
   ```bash
   vercel --prod
   ```

## Out of Scope

The following were not evaluated in this research:

- Docker image configuration
- CI/CD pipeline setup
- Production-scale architecture (multi-region, HA, DR)
