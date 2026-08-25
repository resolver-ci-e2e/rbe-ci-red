---
name: typecheck-scope
description: What tsc --noEmit actually checks in this repo, for anyone (or any agent) making the CI-fix
type: convention
scope: global
updated: '2026-08-25'
captured_sha: 8cb6cdfa12a629064fb6aab25f0cd17897ad325b
sources:
  - tsconfig.json
  - package.json
sources_sha256:
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

The fix expected on PR branches is already spelled out in ../../README.md
and ../../src/greeting.ts:1-6 (assign a number literal at
src/greeting.ts:11). This page covers what the docs don't:

- ../../tsconfig.json sets `"include": ["src"]` — only files under `src/`
  are typechecked by `npm run typecheck`. A fix that adds or edits files
  outside `src/` will not be caught by this script.
- `"strict": true` is what makes `answer: number = 'forty-two'` a compile
  error (TS2322, string not assignable to number) rather than a lint
  warning — it's a genuine type error, not a stylistic one.
- `typecheck` (`tsc --noEmit`) is the only script in ../../package.json.
  There is no `build`, `test`, or `lint` script — don't assume any of those
  exist when reasoning about "CI" for this repo.
