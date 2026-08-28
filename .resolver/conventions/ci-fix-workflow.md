---
name: ci-fix-workflow
description: Rules for touching src/greeting.ts and validating a fix — read before editing the type error
type: convention
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - src/greeting.ts
  - package.json
  - .github/workflows/ci.yml
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
---

The behavioral rules (never fix `main`, PR-branch fixes are expected and use a
number literal, fix PRs are declined/never merged) are already stated in
`../../README.md` and the comment block in `src/greeting.ts:1-6` — see those,
don't restate them here.

Gap not covered by either doc: how to validate a fix locally. There is no
`test` script in `package.json` — the only check CI runs is `npx tsc --noEmit`
(wired up as `npm run typecheck`). Run `npm install && npm run typecheck`
after editing `src/greeting.ts:11`; passing that is sufficient and is the only
signal CI checks.
