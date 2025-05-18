# System Patterns Index

## System Architecture
- Modular memory bank with subfolders: `core/`, `systemPatterns/`, `techContext/`, `progress/`.
- MCP server exposes memory bank as resources and tools, with robust error handling and port failover.
- Webview dashboard for user interaction, including initialisation and updates.
- Service-oriented architecture for extension components (MemoryBankService, MCPServer, CommandHandler, WebviewManager, Extension entry point).
- **MCP server CLI entrypoint for stdio transport implemented for Cursor 0.50+ compatibility (2025-05-11).**

## Key Technical Decisions
- All file operations are async and robust, with readiness checks and error handling.
- Migration logic for flat to modular structure, with user consent.
- Automatic port failover for MCP server.
- Cursor-first compatibility, with VS Code as a bonus.
- Public documentation in `docs/`, private memory in `memory-bank/`.
- Use of Git Flow for release management.
- **CLI-based MCP server and JSON-RPC 2.0 compliance now standard.**

## Design Patterns in Use
- Service-oriented architecture for extension components.
- Command handler for `/memory` commands.
- Separation of concerns between backend, MCP server, and webview.
- Planner tools for extracting and updating project plans (see `EXPERIMENTAL-MCP-PLAN.md`).
- **CLI/stdio transport pattern for MCP server, replacing legacy HTTP/SSE for Cursor 0.50+.**

## Component Relationships (Mermaid)
```mermaid
graph TD
    Extension --> MemoryBankService
    Extension --> MCPServer
    Extension --> WebviewManager
    MCPServer --> MemoryBankService
    WebviewManager --> MCPServer
    CommandHandler --> MCPServer
```

_Last updated: 2025-05-11 🐹_
