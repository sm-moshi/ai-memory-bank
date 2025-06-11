# Development Environment

Facts about the local and CI environments gathered from `package.json`, `mise.toml`, and GitHub Actions workflows.

## Local Tooling

- **Node.js**: 20 (configured via `mise.toml`)
- **pnpm**: 10.x (declared engine in `package.json`)
- **VS Code**: ≥1.96.2 (minimum stated in extension engines)
- **Operating Systems**: macOS (darwin 24.5.0) verified from user info; CI also runs on Ubuntu-latest runners

## Scripts & Commands

- `pnpm dev` – Parallel watch mode (`rollup` + `vite dev`)
- `pnpm build` – Full extension + webview production build
- `pnpm test` – Runs vitest
- `pnpm lint` / `pnpm format` – Biome checks & formatting

## Continuous Integration

- GitHub Actions workflow `ci.yml` executes: install deps → lint → test → build → vsce package

---
> *Last updated: 2025-06-11*
