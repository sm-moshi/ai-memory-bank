# System Architecture

A factual overview of how the AI Memory extension is structured, based on the current source tree (🗂 `src/`) and build outputs (📦 `dist/`).

## High-Level View

| Layer | Key Modules | Responsibilities |
|-------|-------------|-------------------|
| **VS Code / Cursor Extension** | `src/extension.ts`, `src/vscode/**` | Activates on VS Code events, registers commands (`aimemory.*`), embeds webview, launches MCP server adapter. |
| **MCP Server** | `src/mcp/server.ts`, `src/mcp/tools.ts`, `src/mcp/transport.ts` | Provides model-context protocol (MCP) interface over **STDIO**. Hosts tools/resources for the memory bank. Builds to `dist/index.cjs`. |
| **Core Services** | `src/core/*` | File operations (`FileOperationManager`), optional tiered loading (hot/warm/cold), secure memory-bank access (`MemoryBankManager`). |
| **Cursor Integration** | `src/cursor/**`, `src/cursor-integration.ts` | Updates `~/.cursor/mcp.json`, registers Cursor-specific prompts, rules, and commands. |
| **Webview Dashboard** | `src/webview/**` (React 19 + Tailwind 4) | UI for browsing & editing the memory bank via structured `postMessage` channel. Compiled by Vite to `media/` assets included in the VSIX. |

## Component Interaction

```
Extension (VS Code) ─┬─► MemoryBankMCPAdapter ─► MCP Server (STDIO)
                    │
                    └─► WebviewProvider ⇄ Webview UI (React)
                               │
                               └─► MemoryBankManager (via adapter calls)
```

1. When the user runs **"AI Memory: Start MCP Server"**, the extension spawns the in-process MCP server using the adapter.
2. The MCP server exposes **tools** (e.g., `read_file`, `update_file`) that operate on the memory-bank via `MemoryBankManager`.
3. Cursor connects over STDIO (`model_context_protocol`) and can invoke those tools.
4. The webview UI exchanges JSON-validated messages with the extension to read/write memory-bank files.

## Security & Validation

- **Path Validation**: `validateMemoryBankPath` ensures all file accesses remain inside the workspace or explicit `memory-bank/` root.
- **Schema Validation**: Zod schemas guard webview messages (`WebviewFileOperationSchema`).
- **Retry & Back-off**: `FileOperationManager` wraps fs operations with retries, exponential back-off, and time-outs.

## Build & Deployment Outputs

| Artefact | Purpose |
|----------|---------|
| `dist/extension.cjs` | Extension entry loaded by VS Code/Cursor. |
| `dist/index.cjs` | Stand-alone MCP server for Cursor CLI integration. |

---
> _Last updated: 2025-06-11_
