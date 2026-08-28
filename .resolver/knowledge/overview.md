---
name: overview
description: What this repo is (a deliberately-red CI fixture) and the hard rule agents must follow before touching main or PR branches
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - README.md
  - package.json
  - src/greeting.ts
  - .github/workflows/ci.yml
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  README.md: 94e891950db6aae4acb55484a33e18552acbd4aa1f88c834541bec8f45a503b2
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
---

This repo (`rbe-ci-red`) is a journey-suite test fixture, not a product codebase. Its whole purpose is a single deliberate type error on `main` that keeps `ci` red; the full rationale and the "don't fix main / do fix PR branches" rule are stated in [README.md](../../README.md) and repeated as a code comment in `src/greeting.ts:1-6` — read those before making any change here.

Derived facts not stated in the README:
- Only one npm script exists: `typecheck` (`tsc --noEmit`) — there is no test, build, lint, or start script (`package.json`).
- No test framework is present anywhere in the repo; don't assume Jest/Vitest exist.
- TypeScript is pinned to `5.6.3` and `tsconfig.json` runs in `strict` mode with `noEmit`, `module`/`moduleResolution: NodeNext`, targeting `ES2022`.
- CI (`.github/workflows/ci.yml`) pins `node-version: '20'` and runs `npm install --no-audit --no-fund` then `npx tsc --noEmit` — there's no separate lint or test job.

The single content file is `src/greeting.ts`; the failing line is `export const answer: number = 'forty-two';` (line 11), a string assigned to a `number`-typed export.
