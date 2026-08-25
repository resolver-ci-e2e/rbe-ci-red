---
name: overview
description: What this fixture repo is for and how its red/green CI behavior actually works
type: knowledge
scope: global
updated: '2026-08-25'
captured_sha: 8cb6cdfa12a629064fb6aab25f0cd17897ad325b
sources:
  - README.md
  - package.json
  - src/greeting.ts
  - tsconfig.json
sources_sha256:
  README.md: 94e891950db6aae4acb55484a33e18552acbd4aa1f88c834541bec8f45a503b2
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

This repo's purpose, the "do not fix main" rule, and the expected PR-branch
fix are already stated correctly in ../../README.md and in the header
comment at ../../src/greeting.ts:1-6 — read those first.

## Gaps / derived facts not in the docs

- **Re-provision script isn't in this repo.** ../../README.md points to
  `tests/journeys/scripts/provision-ci-red-fixtures.ts` for re-provisioning,
  but no `tests/` directory exists anywhere in this working tree. That script
  lives in the parent resolver-core monorepo, not in `rbe-ci-red` itself — do
  not go looking for it here.
- **No CI workflow lives in this repo.** There is no `.github/workflows/`
  (or any other CI config) in the tree. The `ci` check-run referenced in the
  README is driven externally by resolver-core's journey harness, which
  presumably runs `npm run typecheck` (the only script defined in
  ../../package.json) against this repo — it is not self-hosted here.
- **Single source file, no tests.** `src/greeting.ts` is the only source
  file and there is no test suite. The whole fixture's "red" state reduces to
  one TS2322 error: `answer: number = 'forty-two'` at src/greeting.ts:11.
