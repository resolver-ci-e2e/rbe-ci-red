---
name: typescript-conventions
description: tsconfig strictness and the required shape of the fix for greeting.ts
type: convention
scope: global
updated: 2026-09-15 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - tsconfig.json
  - src/greeting.ts
  - README.md
sources_sha256:
  README.md: 94e891950db6aae4acb55484a33e18552acbd4aa1f88c834541bec8f45a503b2
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

`tsconfig.json` is `strict: true`, `target: ES2022`, `module`/`moduleResolution: NodeNext`,
`noEmit: true`, and only includes `src`. There's no build output — `typecheck`
(`tsc --noEmit`) is the only compile step, on this single-file `src/`.

On PR branches, the expected fix for `src/greeting.ts:11` is narrow: replace
the string literal with a **number literal** (e.g. `42`), not a type
annotation change, cast, or refactor of `answer`'s declared type — see
`../../README.md` for why (that's the exact behavior the CI-fix-agent journey
test asserts). Never touch `main`; fix PRs are declined, never merged.
