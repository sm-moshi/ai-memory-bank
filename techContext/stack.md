# Technology Stack

A concise overview of the primary technologies used in this project. Information sourced directly from `package.json`, `README.md`, and tooling configuration files to ensure accuracy.

## Languages & Runtime

- **TypeScript** – 5.9.x (strict mode)
- **JavaScript runtime** – Node.js 20 (managed via `mise`)
- **Markdown** – for documentation in `memory-bank/` and public docs

## Package & Build Tooling

- **Package manager** – pnpm 10.x
- **Bundler (extension)** – Rollup 4.x
- **Bundler (webview)** – Vite 6.x with `@vitejs/plugin-react`
- **TypeScript build helper** – tsx 4.x
- **SWC** – for fast Rollup transforms (`@rollup/plugin-swc`)

## Frameworks & Libraries

- **React** 19.1 (Concurrent features enabled)
- **Tailwind CSS** 4.1 – utility-first styling for the webview
- **Commander** 14.x – CLI argument parsing
- **Zod** 3.25 – runtime validation

## Testing & Quality

- **Vitest** 3.x – unit/integration tests
- **Happy DOM** 18.x – DOM environment for React tests
- **@testing-library/jest-dom** 6.x – testing utilities
- **Biome** 2.x – formatting & linting (replaces ESLint + Prettier)
- **Rollup Plugin Analyzer** – bundle inspection during CI

## VS Code / Cursor Integration

- Targets **VS Code ≥ 1.96.2** (Cursor compatible)
- Extension commands registered under namespace `aimemory.*`
- Bundled output:
  - `dist/extension.cjs` – VS Code extension entry
  - `dist/index.cjs` – Stand-alone MCP server entry

## Continuous Integration

- GitHub Actions workflow `ci.yml` builds, tests, and packages the extension
- Status badge visible in `README.md`

---
> *Last updated: 2025-06-11*
