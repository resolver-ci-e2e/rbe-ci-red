---
name: architecture
description: Shape of the single CI typecheck pipeline and how it produces the annotations resolver-core consumes
type: knowledge
scope: global
updated: 2026-08-28 (IONE-959)
captured_sha: 81d9d2aff2771b2239d959397f29b77902607392
sources:
  - .github/workflows/ci.yml
  - tsconfig.json
  - package.json
sources_sha256:
  .github/workflows/ci.yml: 78a07cbeb9a9a149e2c4b07d43c1748d898b13cf2fd769e79e06fbb270ae4acb
  package.json: 91e7e08e3d8c1f4faeb295e1404b691497c70e5207f72e671e01e9e661a65bf4
  tsconfig.json: b3433dd8b2ee73252b402dc6964bbd0b8fd3e34802fd0750c24a884501678da9
---

There is no runtime service in this repo — the only "system" is a single GitHub
Actions job that typechecks `src/`.

```mermaid
flowchart LR
    A[push / pull_request] --> B[actions/checkout@v4]
    B --> C[actions/setup-node@v4 node 20]
    C -->|registers tsc problem matcher| D[npm install]
    D --> E[npx tsc --noEmit]
    E -->|type error at src/greeting.ts:11| F[check-run annotation]
    F --> G[resolver-core CI-fix agent]
```

- `setup-node@v4` is what wires up the built-in `tsc` problem matcher (see
  comment in `.github/workflows/ci.yml`); that's the mechanism that turns a
  plain `tsc --noEmit` failure into a `{path, line, message}` check-run
  annotation rather than just red job output — that annotation is the actual
  signal resolver-core's CI-fix seeding reads.
- `on: push branches: ['**']` plus `pull_request` means every branch and every
  PR gets typechecked, including `main` (which is expected to stay red).
