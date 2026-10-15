# Vue Foundation

English · [简体中文](./README.md)

A strict TypeScript starter built on stable Vue 3 (**3.5.44**, the npm `latest` tag at initialization). The landing page introduces the stack, architecture, and a working Pinia example. `pnpm-lock.yaml` records the resolved versions.

## Quick start

Requires **Node.js 22.13+ (22 LTS) or 24+** and **pnpm 10.34.6**. Prefer the latest Node 22 LTS patch.

```sh
corepack enable
corepack install
pnpm install
pnpm dev
```

Open the URL printed in the terminal. Optionally copy `.env.example` to `.env.local`, set `VITE_APP_TITLE`, and restart the dev server. All `VITE_` variables are public browser data; never use them for secrets.

## Stack

- Vue 3 Composition API, `<script setup lang="ts">`, and Vite.
- Vue Router with lazy-loaded views and a 404 page; Pinia setup stores.
- TypeScript `strict`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, and Vue template checking.
- ESLint 10 flat config with type-aware checks and Vue recommended rules; explicit `any` and floating promises are rejected.
- Prettier, EditorConfig, and VS Code format-on-save; formatting and linting have separate responsibilities.
- Vitest, Vue Test Utils, and jsdom for component interaction and derived state tests.
- Local Codex CLI dependency, project configuration, and `AGENTS.md`; GitHub Actions quality and build checks.

## Commands

| Command                             | Purpose                                                   |
| ----------------------------------- | --------------------------------------------------------- |
| `pnpm dev`                          | Start the development server                              |
| `pnpm build`                        | Typecheck and build into `dist/`                          |
| `pnpm preview`                      | Preview the production build locally                      |
| `pnpm typecheck`                    | Check application, Vue templates, and build configuration |
| `pnpm lint` / `pnpm lint:fix`       | Lint with zero warnings / apply fixes                     |
| `pnpm format` / `pnpm format:check` | Format / verify formatting                                |
| `pnpm test` / `pnpm test:watch`     | Run tests / watch mode                                    |
| `pnpm check`                        | Run formatting, lint, type, and test checks               |
| `pnpm codex:login`                  | Authenticate the Codex CLI                                |
| `pnpm codex`                        | Start Codex in this project                               |

## Structure

```text
.codex/config.toml       Shared Codex project defaults
.github/workflows/ci.yml Automated quality checks
.vscode/                Recommended extensions and editor settings
docs/architecture.md    Architecture, comments, environment, deployment
public/                 Static assets served without compilation
src/
  assets/styles/        Global styles and design tokens
  components/           Reusable UI; colocated tests
  composables/          Reusable useX reactive logic
  router/               Routes and lazy loading
  services/             External I/O and runtime validation boundaries
  stores/               Shared application state
  types/                Shared domain types
  utils/                Pure reusable functions
  views/                Route-level components
  App.vue               Root component
  env.d.ts              Environment declarations
  main.ts               Entry point and plugin registration
AGENTS.md               Codex collaboration instructions
```

Add modules when needed instead of speculative request wrappers or business abstractions. As complexity grows, colocate business code under `src/features/<feature>/`. Keep local state in components and shared state in Pinia. Comments explain intent, constraints, and edge cases; use TSDoc for nontrivial public APIs. See [architecture guidance](./docs/architecture.md).

## Codex CLI

The CLI is a locked local development dependency; no global install is required:

```sh
pnpm codex:login
pnpm codex
# Start with a task
pnpm codex "Read AGENTS.md, implement a new page, and run the checks"
```

You must complete ChatGPT or API authentication yourself; installing this starter does not sign you in. Codex reads the root `AGENTS.md` and loads `.codex/config.toml` after you trust the project. Shared defaults use `workspace-write` and `on-request`, preserving your model selection. Local authentication and session files are ignored and must not be committed.

Workflow: describe the task → Codex reads conventions and edits → run `pnpm check` and `pnpm build` → review the diff. Official docs: [CLI](https://developers.openai.com/codex/cli), [AGENTS.md](https://developers.openai.com/codex/guides/agents-md), [project configuration](https://developers.openai.com/codex/config-basic).

## Validation and deployment

```sh
pnpm install --frozen-lockfile
pnpm check
pnpm build
```

CI runs the same checks. Serve `dist/` from a static host, rewriting unmatched URLs to `index.html` for history routing. Configure Vite `base` for subdirectory deployment. `preview` is a local preview server. The landing page loads Google Fonts with system font fallbacks when offline.

## Dependency updates

Inspect updates with `pnpm outdated`, review release notes, then run `pnpm update` and all checks. See the [Vue release policy](https://vuejs.org/about/releases). Commit the pnpm lockfile and avoid mixing package managers.
