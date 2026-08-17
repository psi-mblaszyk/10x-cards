---
bootstrapped_at: 2026-08-17T11:00:00Z
starter_id: 10x-astro-starter
starter_name: "10x Astro Starter (Astro + Supabase + Cloudflare)"
project_name: 10x-cards
language_family: js
package_manager: npm
cwd_strategy: git-clone
bootstrapper_confidence: first-class
phase_3_status: ok
audit_command: "npm audit --json"
---

## Hand-off

```yaml
starter_id: 10x-astro-starter
package_manager: npm
project_name: 10x-cards
hints:
  language_family: js
  team_size: solo
  deployment_target: cloudflare-pages
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: first-class
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: true
  has_background_jobs: false
```

### Why this stack

10xCards is an AI-powered active recall flashcard application being built as a greenfield web-app MVP within a 3-week timeline for a small user scale. A solo developer shipping this product needs an opinionated, highly productive, and agent-friendly stack that provides essential features out of the box. The 10x Astro Starter (`10x-astro-starter`) fits these needs perfectly, combining Astro 6 with React 19, TypeScript, and Tailwind CSS 4, backed by Supabase for authentication and database management, and Cloudflare Pages for edge-native hosting. This starter provides robust support for the application's core requirements—namely built-in authentication (FR-001, FR-002) and an edge-compatible environment ideal for streaming LLM integrations (FR-008, FR-009)—with no unneeded complexity like payments or real-time sync. It clears all four agent-friendly criteria and carries a 'first-class' bootstrapper confidence, ensuring smooth scaffolding and development with the AI agent. The stack is configured with GitHub Actions CI/CD for auto-deployment to Cloudflare Pages on merges to main, aligning with solo developer efficiency and rapid shipping.

## Pre-scaffold verification

| Signal             | Value                              | Severity | Notes                              |
| ------------------ | ---------------------------------- | -------- | ---------------------------------- |
| npm package        | create-astro                       | not run  | skipped npm package check for git-clone strategy |
| GitHub repo        | https://github.com/przeprogramowani/10x-astro-starter last pushed 2026-05-17T10:33:39Z | fresh | active repository |

## Scaffold log

**Resolved invocation**: `git clone https://github.com/przeprogramowani/10x-astro-starter .bootstrap-scaffold && cd .bootstrap-scaffold && npm install`
**Strategy**: git-clone (cloned starter repo)
**Exit code**: 0
**Files moved**: 20 root-level items (including hidden directories and config files)
**Conflicts (.scaffold siblings)**: none
**.gitignore handling**: moved silently
**.bootstrap-scaffold cleanup**: deleted

## Post-scaffold audit

**Tool**: npm audit --json
**Summary**: 1 CRITICAL, 13 HIGH, 7 MODERATE, 2 LOW
**Direct vs transitive**: 0/1/2/0 direct of total 1/13/7/2

### CRITICAL findings

#### `tar` (Transitive)
- **Severity**: critical
- **Description**: node-tar is vulnerable to decompression/parse denial of service (DoS) via unlimited input (CVE-2024-436) and recursion stack-overflow.
- **Advisory ID**: GHSA-23hp-3jrh-7fpw
- **Affected package**: `tar` (used by `supabase` client)
- **Fix version**: >= 7.5.18

### HIGH findings

#### `astro` (Direct)
- **Severity**: high
- **Description**: Reflected XSS via unescaped slot name (CVE-2026-54298) and SSRF via Host header.
- **Advisory ID**: GHSA-8hv8-536x-4wqp, GHSA-2pvr-wf23-7pc7
- **Affected package**: `astro`
- **Fix version**: >= 6.4.6

#### `brace-expansion` (Transitive)
- **Severity**: high
- **Description**: DoS via exponential-time expansion of consecutive non-expanding `{}` groups and unbounded expansion length.
- **Advisory ID**: GHSA-3jxr-9vmj-r5cp, GHSA-mh99-v99m-4gvg, GHSA-rgw5-rvv9-x895
- **Affected package**: `brace-expansion`
- **Fix version**: >= 1.1.18

#### `devalue` (Transitive)
- **Severity**: high
- **Description**: Svelte devalue is vulnerable to DoS via sparse array deserialization.
- **Advisory ID**: GHSA-77vg-94rm-hx3p
- **Affected package**: `devalue`
- **Fix version**: >= 5.8.1

#### `fast-uri` (Transitive)
- **Severity**: high
- **Description**: fast-uri is vulnerable to host confusion via backslash authority introducer/delimiter.
- **Advisory ID**: GHSA-v2hh-gcrm-f6hx, GHSA-7p8r-x3mc-p8w7
- **Affected package**: `fast-uri`
- **Fix version**: >= 3.1.5

#### `js-yaml` (Transitive)
- **Severity**: high
- **Description**: js-yaml: YAML merge-key chains can force quadratic CPU consumption leading to DoS.
- **Advisory ID**: GHSA-52cp-r559-cp3m, GHSA-5p4m-2wfm-xmqj
- **Affected package**: `js-yaml`
- **Fix version**: >= 4.3.1

#### `nanoid` (Transitive)
- **Severity**: high
- **Description**: custom generators can loop indefinitely when size is zero or negative size.
- **Advisory ID**: GHSA-28wg-ghj8-5hjv, GHSA-2v37-7h3g-55p8
- **Affected package**: `nanoid`
- **Fix version**: >= 3.3.18

#### `postcss` (Transitive)
- **Severity**: high
- **Description**: Path Traversal in Previous Source Map Auto-Loading leads to Arbitrary .map File Disclosure.
- **Advisory ID**: GHSA-r28c-9q8g-f849
- **Affected package**: `postcss`
- **Fix version**: >= 8.5.18

#### `sharp` (Transitive)
- **Severity**: high
- **Description**: sharp inherited vulnerabilities in libvips (CVE-2026-33327, CVE-2026-33328, CVE-2026-35590, CVE-2026-35591).
- **Advisory ID**: GHSA-f88m-g3jw-g9cj
- **Affected package**: `sharp`
- **Fix version**: >= 0.35.0

#### `svgo` (Transitive)
- **Severity**: high
- **Description**: SVGO removeScripts plugin leaves some executable scripts intact.
- **Advisory ID**: GHSA-2p49-hgcm-8545
- **Affected package**: `svgo`
- **Fix version**: >= 4.0.2

#### `undici` (Transitive)
- **Severity**: high
- **Description**: WebSocket client vulnerable to denial of service, cookie attribute injection, CRLF injection, and cross-origin request routing via SOCKS5 proxy pool reuse.
- **Advisory ID**: GHSA-vmh5-mc38-953g, GHSA-vxpw-j846-p89q, GHSA-hm92-r4w5-c3mj, GHSA-4cwx-7wf7-3272
- **Affected package**: `undici`
- **Fix version**: >= 7.29.0

#### `vite` (Transitive)
- **Severity**: high
- **Description**: `server.fs.deny` bypass on Windows alternate paths leading to arbitrary file disclosure.
- **Advisory ID**: GHSA-fx2h-pf6j-xcff
- **Affected package**: `vite`
- **Fix version**: >= 7.3.5

#### `ws` (Transitive)
- **Severity**: high
- **Description**: ws is vulnerable to memory exhaustion DoS from tiny fragments and data chunks.
- **Advisory ID**: GHSA-96hv-2xvq-fx4p
- **Affected package**: `ws`
- **Fix version**: >= 8.21.0

### MODERATE findings

#### `supabase` (Direct)
- **Severity**: moderate
- **Description**: Contains a transitive dependency `tar` that is vulnerable.
- **Advisory ID**: GHSA-vmf3-w455-68vh, GHSA-w8wr-v893-vjvp
- **Affected package**: `supabase`
- **Fix version**: Upgrade dependencies or configure custom path.

#### `wrangler` (Direct)
- **Severity**: moderate
- **Description**: Contains transitive vulnerabilities via dependencies `esbuild` and `miniflare`.
- **Advisory ID**: GHSA-g7r4-m6w7-qqqr, GHSA-fx2h-pf6j-xcff
- **Affected package**: `wrangler`
- **Fix version**: Upgrade wrangler or sub-dependencies.

#### `@astrojs/language-server` (Transitive)
- **Severity**: moderate
- **Description**: Vulnerable via `volar-service-yaml`.
- **Affected package**: `@astrojs/language-server`

#### `@cloudflare/vite-plugin` (Transitive)
- **Severity**: moderate
- **Description**: Vulnerable via `miniflare`, `wrangler`, and `ws`.
- **Affected package**: `@cloudflare/vite-plugin`

#### `volar-service-yaml` (Transitive)
- **Severity**: moderate
- **Description**: Vulnerable via `yaml-language-server`.
- **Affected package**: `volar-service-yaml`

#### `yaml` (Transitive)
- **Severity**: moderate
- **Description**: Vulnerable to Stack Overflow via deeply nested YAML collections.
- **Advisory ID**: GHSA-48c2-rrv3-qjmp
- **Affected package**: `yaml`
- **Fix version**: >= 2.8.3

#### `yaml-language-server` (Transitive)
- **Severity**: moderate
- **Description**: Vulnerable via `yaml`.
- **Affected package**: `yaml-language-server`

### LOW / INFO findings

#### `@babel/core` (Transitive)
- **Severity**: low
- **Description**: Arbitrary File Read via sourceMappingURL Comment.
- **Advisory ID**: GHSA-4x5r-pxfx-6jf8
- **Affected package**: `@babel/core`
- **Fix version**: >= 7.29.1

#### `esbuild` (Transitive)
- **Severity**: low
- **Description**: Allows arbitrary file read when running development server on Windows.
- **Advisory ID**: GHSA-g7r4-m6w7-qqqr
- **Affected package**: `esbuild`
- **Fix version**: >= 0.28.1

## Hints recorded but not acted on

| Hint                       | Value                              |
| -------------------------- | ---------------------------------- |
| bootstrapper_confidence    | first-class                        |
| quality_override           | false                              |
| path_taken                 | standard                           |
| team_size                  | solo                               |
| deployment_target          | cloudflare-pages                   |
| ci_provider                | github-actions                     |
| ci_default_flow            | auto-deploy-on-merge               |
| has_auth                   | true                               |
| has_payments               | false                              |
| has_realtime               | false                              |
| has_ai                     | true                               |
| has_background_jobs        | false                              |

## Next steps

Next: a future skill will set up agent context (CLAUDE.md, AGENTS.md). For now, your project is scaffolded and verified — happy hacking.

Useful manual steps in the meantime:
- `git init` (if you have not already) to start your own repo history.
- Review any `.scaffold` siblings the conflict policy created and decide which version of each file to keep.
- Address audit findings per your project's risk tolerance — the full breakdown is in this log.
