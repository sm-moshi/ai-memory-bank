# Design Patterns

Catalogue of concrete design & architectural patterns observed in the current codebase. Patterns are only listed where they are clearly identifiable from the source.

| Pattern | Location / Example | Purpose |
|---------|--------------------|---------|
| **Adapter** | `src/vscode/mcp-adapter.ts` (`MemoryBankMCPAdapter`) | Bridges VS Code extension API with the generic MCP server. |
| **Manager** | `FileOperationManager`, `MemoryBankManager`, `StreamingManager` | Encapsulate complex operations behind cohesive APIs. |
| **Factory (simple)** | `createLogger` in `src/lib/logging.ts` | Centralised construction of logger instances with environment detection. |
| **Command** | VS Code commands registered in `src/extension.ts` (`aimemory.*`) | Encapsulate user-triggered actions behind command IDs. |
| **Facade** | `src/mcp/server.ts` (`BaseMCPServer`) | Provides a simplified interface over underlying tools/resources. |
| **Singleton (per process)** | Logger instance returned by `createLogger()` when running inside the extension context | Ensures one output channel across the extension runtime. |

## Architectural Guidelines Employed

- **KISS** & **DRY**: Minimal abstractions, clear separation between extension, core, and webview.
- **SOLID** (selectively):
  - *Single Responsibility*: Managers focus on one domain (e.g., file IO vs. streaming).
  - *Open/Closed*: Core services expose public APIs; new behaviours are added via composition rather than modification.

---
> *Last updated: 2025-06-11*
