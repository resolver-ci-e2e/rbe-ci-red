---
name: architecture
description: Shape of the CI pipeline that makes this fixture red — read before touching the workflow or the typecheck step
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - .github/workflows/ci.yml
  - package.json
  - src/greeting.ts
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
---

There's no service/module graph here — the only "system" is the single GitHub
Actions pipeline that turns the type error in `src/greeting.ts` into a failing
check-run.

```mermaid
flowchart LR
  P[push: any branch] --> W[ci workflow]
  PR[pull_request] --> W
  W --> J[typecheck job]
  J --> CO[actions/checkout]
  CO --> SN["actions/setup-node (registers tsc problem matcher)"]
  SN --> NI[npm install]
  NI --> TC[npx tsc --noEmit]
  TC -->|error at src/greeting.ts:11| AN["check-run annotation {path, line, message}"]
```

The `setup-node` step is what converts a raw `tsc` failure into a structured
annotation (`{path, line, message}`) rather than just a red job — that
annotation is the signal the resolver-core CI-fix agent seeding consumes, per
the comment in `.github/workflows/ci.yml`.
