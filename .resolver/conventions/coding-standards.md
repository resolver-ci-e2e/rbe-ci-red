---
name: coding-standards
description: TypeScript strictness and the one intentional exception (src/greeting.ts:11) — read before editing src/
type: convention
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - tsconfig.json
  - src/greeting.ts
sources_sha256:
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

`tsconfig.json` has `strict: true`, target `ES2022`, module/moduleResolution
`NodeNext`, `noEmit: true`. Under strict mode a plain string can't be assigned
to a `number`-typed const — which is exactly why line 11 of `src/greeting.ts`
fails today; it isn't a lint-config gap, it's the deliberate fixture bug.

- On `main`: never change `export const answer: number = 'forty-two';` — see
  [../../README.md](../../README.md).
- On a PR branch: the expected minimal fix is replacing `'forty-two'` with a
  number literal (the variable name `answer` strongly implies `42`, though no
  file pins that value explicitly) — do not change the declared `: number`
  type or widen it to `any`/`string`, since that would defeat the fixture's
  purpose of testing a real type-mismatch fix.
- No ESLint/Prettier config exists in this repo — don't introduce one as part
  of an unrelated fix.
