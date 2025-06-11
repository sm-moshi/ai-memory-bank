# Dependencies Overview

A snapshot of key runtime and development dependencies. Version numbers reflect those currently found in `package.json` on the **develop** branch.

## Runtime Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `@modelcontextprotocol/sdk` | ^1.12.1 | MCP server tooling |
| `commander` | ^14.0.0 | CLI argument parsing |
| `gray-matter` | ^4.0.3 | Front-matter parsing for markdown files |
| `react` | ^19.1.0 | Webview UI framework |
| `react-dom` | ^19.1.0 | React DOM renderer |
| `zod` | ^3.25.58 | Runtime schema validation |

## Development Dependencies (partial)

| Package | Version | Role |
|---------|---------|------|
| `@biomejs/biome` | ^2.0.0-beta.6 | Linting & formatting |
| `rollup` | ^4.42.0 | Bundling VS Code extension |
| `vite` (transitive) | 6.3.5 via `vite-tsconfig-paths` & plugin-react | Webview dev server/bundler |
| `vitest` | ^3.2.3 | Testing framework |
| `@vitejs/plugin-react` | ^4.5.2 | React SWC transform for Vite |
| `@types/vscode` | 1.96.0 | VS Code API typings |
| `@vsce/test-cli` | ^0.0.11 | Extension test runner |
| `@rollup/plugin-alias` | ^5.1.1 | Path aliasing for Rollup |
| `@rollup/plugin-swc` | ^0.4.0 | SWC transform for Rollup |

_For a full list, refer to `package.json`._

---
> _Last updated: 2025-06-11_
