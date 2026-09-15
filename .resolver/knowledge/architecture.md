---
name: architecture
description: Shape of the CI pipeline that produces the intentional red build
type: knowledge
scope: global
updated: 2026-09-15 (IONE-959)
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

There's no runtime service here — the only "system" is the single CI job and
the one source file it typechecks.

```mermaid
flowchart LR
    A[push: any branch] --> C[job: typecheck]
    B[pull_request] --> C
    C --> D[actions/checkout@v4]
    D --> E[actions/setup-node@v4\nnode 20, tsc problem matcher]
    E --> F[npm install --no-audit --no-fund]
    F --> G[npx tsc --noEmit]
    G --> H[src/greeting.ts:11\nanswer: number = 'forty-two']
    H -- fails on main --> I[check-run annotation]
    I --> J[resolver-core CI-fix agent\nconsumes annotation on PR branch]
```

The `setup-node` step registering the `tsc` problem matcher is why the
`tsc --noEmit` failure surfaces as a structured `{path, line, message}`
check-run annotation rather than plain log text — that's the specific signal
the resolver-core agent seeding reads, per the comment in
`.github/workflows/ci.yml`.
