---
name: overview
description: What this repo is (a resolver-core CI-fix journey fixture) and where the rules live
type: knowledge
scope: global
updated: 2026-09-15 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - package.json
  - README.md
  - src/greeting.ts
  - .github/workflows/ci.yml
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  README.md: 94e891950db6aae4acb55484a33e18552acbd4aa1f88c834541bec8f45a503b2
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
---

Single-package TypeScript fixture (`rbe-ci-red`, one file: `src/greeting.ts`).
It exists to keep `main` deterministically red in CI so resolver-core's
CI-fix-agent journey tests have a known failure to consume and fix on a PR
branch. See `../../README.md` for the full intent and the "do not fix main /
fix PRs are declined, never merged" rules — that doc is authoritative and
current, do not restate it here.

Gaps not covered by the README:
- Only script defined is `typecheck` (`tsc --noEmit`). There is no lint,
  format, build, or test script anywhere in `package.json`.
- No `package-lock.json` is committed, so CI (`.github/workflows/ci.yml`)
  runs `npm install`, not `npm ci` — dependency resolution isn't lockfile-pinned
  run-to-run, even though `typescript` is pinned exact (`5.6.3`) in
  `devDependencies`.
- No `.eslintrc`/`.prettierrc`/`.cursor` config exists; `tsc --strict` is the
  only enforced check in this repo.
