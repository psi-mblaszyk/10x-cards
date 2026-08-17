---
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
---

## Why this stack

10xCards is an AI-powered active recall flashcard application being built as a greenfield web-app MVP within a 3-week timeline for a small user scale. A solo developer shipping this product needs an opinionated, highly productive, and agent-friendly stack that provides essential features out of the box. The 10x Astro Starter (`10x-astro-starter`) fits these needs perfectly, combining Astro 6 with React 19, TypeScript, and Tailwind CSS 4, backed by Supabase for authentication and database management, and Cloudflare Pages for edge-native hosting. This starter provides robust support for the application's core requirements—namely built-in authentication (FR-001, FR-002) and an edge-compatible environment ideal for streaming LLM integrations (FR-008, FR-009)—with no unneeded complexity like payments or real-time sync. It clears all four agent-friendly criteria and carries a 'first-class' bootstrapper confidence, ensuring smooth scaffolding and development with the AI agent. The stack is configured with GitHub Actions CI/CD for auto-deployment to Cloudflare Pages on merges to main, aligning with solo developer efficiency and rapid shipping.
