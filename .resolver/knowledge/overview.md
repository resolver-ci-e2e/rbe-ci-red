---
name: overview
description: What this repo is and why main is deliberately red — read before touching src/greeting.ts or CI
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - package.json
  - README.md
  - src/greeting.ts
sources_sha256:
  README.md: 94e891950db6aae4acb55484a33e18552acbd4aa1f88c834541bec8f45a503b2
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
---

This is a **journey-suite fixture** for resolver-core (spec 014 / j85), not a real
product. Its entire purpose is to keep `main` deterministically red in CI so that
downstream CI-fix-agent tests have a known failure to react to. Full intent and
rules are in [../../README.md](../../README.md) — read it first.

## Gap not covered by README

- The repo has no lockfile (`package-lock.json` absent) and no test framework —
  the only script is `typecheck` (`tsc --noEmit`). Don't assume Jest/Vitest/etc.
  exist; there's nothing to add tests to here, this is a single-purpose fixture.
- The `tests/journeys/scripts/provision-ci-red-fixtures.ts` re-provision script
  the README points to does **not** live in this repo — it lives in the parent
  resolver-core monorepo that generates/seeds this fixture. Don't search for it
  here.
