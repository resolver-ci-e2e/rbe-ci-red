---
name: ci-fix-scope
description: Constraints on the only valid fix for this fixture's seeded type error
type: convention
scope:
  - src/**
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - src/greeting.ts
  - tsconfig.json
sources_sha256:
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

On a PR branch, the fix for the seeded error at `src/greeting.ts:11` (see ../../README.md) is
narrowly constrained by the surrounding code, not just "make tsc pass":

- `answer` is explicitly typed `: number`, and `tsconfig.json` has `strict: true` — so the fix must
  be a numeric literal assigned to that same binding. Do not change the declared type to `string`/
  `any`, add a type assertion (`as number`), or use `// @ts-ignore`/`// @ts-expect-error`; those
  satisfy `tsc --noEmit` without matching the fixture's intended single-line numeric-literal fix.
- Don't touch `greeting()` or add new exports/files — the fixture's surface area is intentionally
  just the one binding, and `tsconfig.json`'s `include` is scoped to `src` only.
