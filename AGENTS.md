# Project guidance for Codex

## Stack and commands

- Vue 3 Composition API with `<script setup lang="ts">`, Vite, strict TypeScript, Vue Router, Pinia.
- Use pnpm 10 only. Install with `pnpm install`; preserve `pnpm-lock.yaml`.
- Develop: `pnpm dev`. Before completing changes: `pnpm check` and `pnpm build`.
- `pnpm lint:fix` and `pnpm format` apply automated fixes.

## Architecture

- `src/views`: route-level views, lazy-loaded through `src/router`.
- `src/components`: reusable UI; typed props/emits and accessible HTML.
- `src/composables`: reusable reactive behavior, named `useX`.
- `src/stores`: shared state only; keep transient UI state inside components.
- `src/services`: external I/O; validate unknown data at the boundary.
- `src/types`: shared domain types. Keep local types beside their consumer.
- `src/utils`: pure helpers. Tests live beside implementations as `*.spec.ts`.
- Do not create abstractions or dependencies without a concrete need.

## Code conventions

- Respect strict typing. Avoid `any`, unsafe casts, non-null assertions and suppression comments.
- Use type-only imports. Handle rejected promises and nullable/indexed values explicitly.
- Comment public APIs with concise TSDoc when intent is not obvious; explain decisions and edge cases, not syntax.
- Follow existing Prettier/ESLint configuration. Do not weaken checks to make a task pass.
- Add behavioral tests for new logic and regressions; prefer user-visible outcomes.
- Keep README.md and README.en.md aligned when commands or architecture change.
- Never commit credentials, local Codex auth, or private environment values. VITE_ variables are public.
- Report changes and validation accurately. Do not claim checks ran when they did not.
