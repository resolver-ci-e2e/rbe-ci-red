---
name: overview
description: What this repo is and how it's structured — read first for any task here
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - package.json
  - tsconfig.json
  - src/greeting.ts
sources_sha256:
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

This is a deliberately-red CI fixture, not a real application. See `../../README.md`
for the fixture's purpose and the core rule set (main must stay red; PR-branch
CI-fix agents are expected to make it pass; fix PRs are declined, never merged).

Derived facts not stated in the README:

- The entire source tree is one file, `src/greeting.ts`. There is no other app
  code, no test suite, and no lint script — `package.json` defines only
  `typecheck` (`tsc --noEmit`).
- `tsconfig.json` has `strict: true`, `target: ES2022`, and
  `module`/`moduleResolution: NodeNext`. Any fix to `src/greeting.ts` must
  stay strict-mode valid (e.g. the broken `answer` field needs a real
  `number`, not just anything that happens to compile under looser settings).
