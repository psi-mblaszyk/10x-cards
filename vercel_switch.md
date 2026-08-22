# 10xCards MVP: Cloudflare to Vercel Migration Plan

This plan details the steps required to transition the **10xCards** MVP web-app configuration from Cloudflare Pages to Vercel.

Since the project does not rely on proprietary Cloudflare API bindings (e.g., KV namespaces, D1 databases, or Durable Objects), the migration consists of swapping the Astro adapter package, updating the configuration imports, and adjusting local tooling.

---

## 1. Remove Cloudflare Integration

Remove Cloudflare Pages build adapters and command-line interfaces from the project dependencies:

```bash
npm uninstall @astrojs/cloudflare wrangler
```

Delete the Cloudflare-specific wrangler configuration file:

```bash
rm wrangler.jsonc
```

---

## 2. Install and Configure Vercel Integration

To integrate Vercel, run Astro's automated configuration tool:

```bash
npx astro add vercel
```

### Manual Configuration Alternative

If running the automated installer is not preferred, follow these manual steps:

1. **Install the Vercel Adapter**:

   ```bash
   npm install @astrojs/vercel
   ```

2. **Modify `astro.config.mjs`**:
   Replace the Cloudflare adapter with the Vercel adapter:

   ```javascript
   // @ts-check
   import { defineConfig, envField } from "astro/config";

   import react from "@astrojs/react";
   import sitemap from "@astrojs/sitemap";
   import tailwindcss from "@tailwindcss/vite";
   import vercel from "@astrojs/vercel"; // 1. Swapped import

   // https://astro.build/config
   export default defineConfig({
     output: "server",
     integrations: [react(), sitemap()],
     vite: {
       plugins: [tailwindcss()],
     },
     adapter: vercel(), // 2. Swapped adapter call
     env: {
       schema: {
         SUPABASE_URL: envField.string({ context: "server", access: "secret", optional: true }),
         SUPABASE_KEY: envField.string({ context: "server", access: "secret", optional: true }),
       },
     },
   });
   ```

---

## 3. CI/CD & Deploy Workflow

- **GitHub Actions Pipeline**: The existing CI flow (`.github/workflows/ci.yml`) is already platform-agnostic and only runs validation steps (`npm run lint` and `npm run build`). No modifications are necessary in the workflow file.
- **Production Deploys**: Production deployment is automatically managed via direct Git repository linking in the Vercel Dashboard, mirroring your previous Cloudflare Pages setup.

---

## 4. Local Development & Secrets Wiring

1. **Install Vercel CLI Globally**:

   ```bash
   npm install -g vercel
   ```

2. **Link Project & Provision Secrets**:
   Run the setup wizard from the project root and log into your account:

   ```bash
   vercel login
   vercel
   ```

   Add your existing `.env` secrets (`SUPABASE_URL` and `SUPABASE_KEY`) into the **Vercel Dashboard > Project Settings > Environment Variables** tab to enable dynamic SSR processing.

3. **Parity Local Development**:
   Launch Vercel's edge-replicated dev engine locally:
   ```bash
   vercel dev
   ```
