# Architecture: Internal Modular Memory Bank & MCP CLI/stdio Transport

This document describes the internal, modular, and development-focused architecture for the AI Memory extension's memory bank. It supports robust, context-aware AI workflows in Cursor and VS Code, and is designed for ongoing development, testing, and future enhancements.

## Key Principles
- **Self-healing:** All required files are auto-created from templates if missing, ensuring a valid memory bank at all times.
- **Modular structure:** Clear separation of project, system, tech, and progress context.
- **CLI/stdio transport:** MCP server runs as a CLI/stdio process for robust, context-agnostic integration with Cursor 0.50+.
- **Webview repair:** Manual "Repair Memory Bank" button in the webview for user-triggered self-healing.
- **Cursor-first:** Designed for Cursor, with VS Code compatibility as a bonus.

## Implementation Highlights
- All extension/server components call `loadFiles()` before any operation.
- The memory bank is always kept in a valid, usable state.
- The webview UI provides a "Repair Memory Bank" button.
- MCP server can be started as a CLI/stdio process.

## Benefits
- Prevents blocking or failures due to missing files.
- Improves user experience with clear feedback and easy recovery.
- Enables robust, context-agnostic integration with Cursor and other tools.

_Last updated: 2025-05-18_
