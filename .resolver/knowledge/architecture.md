---
name: architecture
description: The repo's only real "system" is its single-job CI typecheck pipeline
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - .github/workflows/ci.yml
  - package.json
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
---

There is no service/module architecture here — one TS file, one CI job.

```mermaid
flowchart LR
    A[push to any branch\nor pull_request] --> B[job: typecheck]
    B --> C[actions/checkout@v4]
    C --> D[actions/setup-node@v4\nnode 20, tsc problem matcher]
    D --> E[npm install --no-audit --no-fund]
    E --> F[npx tsc --noEmit]
    F -->|src/greeting.ts:11 error on main| G[check-run annotation\nconsumed by CI-fix agent seeding]
```

- The `typecheck` job is the only job in `ci.yml`; it triggers on pushes to every branch (`'**'`) and
  on all pull requests.
- `setup-node@v4` registers the TSC problem matcher before the compile step, which is why `tsc`
  failures surface as structured `{path, line, message}` check-run annotations rather than plain log
  text — that's the signal downstream CI-fix tooling reads.
