# System Patterns Index

> Summary of architectural patterns, design decisions, and system structure

## Architecture Overview
<!-- High-level system design -->
- **Pattern**: Layered architecture (Extension ⇄ MCP Server ⇄ Core Services)
- **Structure**: Source organised by responsibility (`extension`, `mcp`, `core`, `webview`, `cursor`)
- **Communication**: STDIO (MCP) between Cursor and server; postMessage between webview and extension

## Key Design Patterns
<!-- Primary patterns used throughout the system -->
- **Adapter**: Bridges VS Code APIs to generic MCP server (`MemoryBankMCPAdapter`)
- **Manager**: `FileOperationManager`, `MemoryBankManager`, `StreamingManager`
- **Facade**: `BaseMCPServer` aggregates tool/resource registration

## Service Architecture
<!-- How services are structured and interact -->
- **MCP Server**: Serves memory-bank files & tools over STDIO
- **Webview Dashboard**: React UI for memory-bank editing
- **Cursor Integration**: Updates `.cursor/mcp.json` and injects rules/prompts

## Cross-Cutting Concerns
<!-- How the system handles common concerns -->
- **Error Handling**: Result<T,E> pattern & VS Code error notifications
- **Logging**: Unified logger (`createLogger`) with dynamic output routing
- **Configuration**: `vscode.workspace.getConfiguration('aimemory')` + `cursor/config.ts`
- **Security**: Path validation (`validateMemoryBankPath`) & CSP for webview
- **Performance**: Lazy loading (dynamic imports) & streaming for large files

## Quality Patterns
<!-- Patterns ensuring code quality and maintainability -->
- **Testing Strategy**: `vitest` with Happy DOM for React, mocks for VS Code API
- **Build Pipeline**: GitHub Actions (`ci.yml`) → Rollup bundle → Vite build
- **Code Standards**: Biome for linting/formatting, British English enforced
- **Documentation**: Memory-bank + `docs/` for guides

## Reference Links
<!-- Links to detailed documentation -->
- [Architecture Details](architecture.md)
- [Pattern Details](patterns.md)
- [Scanning Patterns](scanning.md)

---
> _Last updated: 2025-06-11_
