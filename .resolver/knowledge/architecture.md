---
name: architecture
description: The repo's real shape — a single-file package whose only "system" is the GitHub Actions typecheck job
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - .github/workflows/ci.yml
  - package.json
  - src/greeting.ts
  - tsconfig.json
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  src/greeting.ts: a8e1dbd805e47e32ce412d18ba9f9c04a46eb1d2a919f3bfb26358735d3bc7f8
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

There is no multi-service architecture here — the entire "system" is one TypeScript module and the CI job that typechecks it.

```mermaid
flowchart LR
    A[push or pull_request] --> B["ci workflow (.github/workflows/ci.yml)"]
    B --> C[actions/checkout]
    C --> D["actions/setup-node (node 20)"]
    D --> E["npm install"]
    E --> F["npx tsc --noEmit"]
    F --> G["src/greeting.ts"]
    F -- "type error → check-run annotation" --> H[resolver-core CI-fix agent seeding]
```

`tsc`'s annotations (not a custom reporter) are the signal consumed downstream: `setup-node` registers the built-in `tsc` problem matcher, which is what turns the compiler error at `src/greeting.ts:11` into a `{path, line, message}` check-run annotation (see the comment in `.github/workflows/ci.yml:14-16`).
