---
name: fix-scope
description: Scope rules for the expected PR-branch CI fix in this fixture — read before touching anything besides the one type error
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

[README.md](../../README.md) and the header comment in `src/greeting.ts` already state the core rule: never fix `main`, and on a PR branch the expected fix is a number literal for `answer` at `src/greeting.ts:11`.

Derived scope constraints, not stated elsewhere: `tsc --noEmit` (via `npm run typecheck` or the `ci` workflow) is the only check in this repo, and `src/greeting.ts:11` is the *only* type error it reports — so the minimal, correct fix touches only that one line. There is no reason for a CI-fix agent to:
- change the `greeting` function's signature or body,
- edit `tsconfig.json`, `package.json`, or `.github/workflows/ci.yml`,
- add tests or a test framework (none exists in this repo), or
- change the type annotation on `answer` instead of its value (the annotation `number` is the intended contract; only the assigned literal is wrong).
