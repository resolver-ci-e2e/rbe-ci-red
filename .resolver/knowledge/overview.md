---
name: overview
description: What rbe-ci-red is and how its single source file/CI job fit together
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - package.json
  - tsconfig.json
  - src/greeting.ts
  - .github/workflows/ci.yml
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

`rbe-ci-red` is a minimal fixture repo (resolver-core spec 014/j85), not an application. Its entire
purpose and the fix-on-PR-branch/never-fix-main rule are documented in ../../README.md — read that
first.

## Structure
- `src/greeting.ts` — the only source file. Exports `greeting()` (untyped-error-free) and
  `answer: number` (deliberately assigned a string literal, the seeded type error).
- `package.json` — single script `typecheck` (`tsc --noEmit`), single devDependency
  (`typescript@5.6.3`). No test runner, no lint config, no build step exist in this repo.
- `tsconfig.json` — `strict: true`, `noEmit: true`, `target: ES2022`, `module`/`moduleResolution:
  NodeNext`. `include` is `["src"]` only.

## Gaps vs README
The README's "Re-provision" pointer (`tests/journeys/scripts/provision-ci-red-fixtures.ts`) does not
exist in this checkout — it lives in the parent resolver-core monorepo, not in this fixture repo.
