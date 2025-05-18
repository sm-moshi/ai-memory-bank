# Dependencies

## Backend (Extension & MCP Server)
- **@modelcontextprotocol/sdk** — Implements the Model Context Protocol and provides memory bank tools.
- **zod** — TypeScript-first schema validation and parsing.
- **express** — Lightweight HTTP server (being removed in favour of stdio transport).
- **cors** — Cross-origin resource sharing middleware (being removed).

## Frontend (Webview)
- **react** — Core UI library for building interactive interfaces.
- **react-dom** — React DOM rendering for webview.
- **react-icons** — Icon library for consistent UI.
- **@tailwindcss/vite** — Tailwind CSS integration for Vite.
- **tailwindcss** — Utility-first CSS framework for styling.
- **@vscode-elements/elements** — VS Code UI elements for webview.
- **@vscode-elements/react-elements** — React bindings for VS Code UI elements.

## Dev & Tooling
- **esbuild** — Fast bundler for extension backend and MCP server.
- **vite** — Bundler and dev server for webview.
- **typescript** — Type safety and maintainability.
- **eslint** — Linting for code quality.
- **vitest** — Unit and integration testing framework.
- **@vscode/test-cli** — Official VS Code extension test runner.
- **pnpm** — Monorepo package management.
- **Justfile** — Task automation (see project root).

---

_Last updated: 2025-05-18_
