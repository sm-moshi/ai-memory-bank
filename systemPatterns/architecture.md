# Architecture: Self-Healing Memory Bank & MCP CLI/stdio Transport (Updated)

## Problem

If any required memory bank file is missing or only partially initialised, the extension or MCP server may block, hang, or fail to start. This is due to strict expectations about the modular file structure and lack of robust error handling for missing files.

Additionally, legacy HTTP/SSE transport for the MCP server was tightly coupled to VS Code and less robust for Cursor-first workflows.

## Solution: Proactive Self-Healing and Robust, Context-Agnostic Transport

- **Proactive Self-Healing:**
  - Before any memory bank operation (read/write/status), the extension now always calls `memoryBankService.loadFiles()`.
  - This ensures all required files are present, auto-creating any missing ones from templates.
  - If repair fails, a clear error is surfaced to the user.
- **User Feedback:**
  - If files were missing and created, a log entry and (optionally) a user notification are generated.
  - If repair fails, the user is told which file(s) could not be created and why.
- **UI Repair Action:**
  - A "Repair Memory Bank" button is now available in the webview, allowing users to manually trigger self-healing. (Implemented 2025-05-11)
- **Granular Error Reporting:**
  - All errors are logged and surfaced to the user with actionable messages.

### MCP CLI/stdio Transport for Cursor 0.50+
- **Why:**
  - The CLI/stdio entrypoint enables context-agnostic, robust integration with Cursor, decoupling the MCP server from HTTP/SSE and VS Code specifics.
  - This approach is more reliable for headless, automated, or multi-environment workflows.
- **How:**
  - The MCP server can now be started as a CLI process, communicating via stdio (JSON-RPC 2.0), as well as HTTP/SSE for legacy support.
  - This is the recommended integration for Cursor 0.50+ and future-proof for other LLM/agent frameworks.
- **Migration:**
  - Legacy HTTP/SSE endpoints are being phased out in favour of CLI/stdio transport.
  - The CLI/stdio entrypoint is now the default for Cursor-first operation.

## Implementation

- MCP server, webview manager, and command handler now call `loadFiles()` before any operation.
- The memory bank is always kept in a valid, usable state, and users are never left with a broken or partially initialised memory bank.
- The webview UI provides a "Repair Memory Bank" button that calls the repair tool and displays feedback to the user.
- The MCP server can be started as a CLI/stdio process for robust, context-agnostic integration with Cursor.

## Benefits

- Prevents blocking, hanging, or silent failures due to missing files.
- Improves user experience with clear feedback and easy recovery.
- Ensures the memory bank is always in a valid, usable state.
- Enables robust, context-agnostic integration with Cursor and other tools via CLI/stdio transport.

## Next Steps

- Continue to improve error feedback and documentation.
- Complete migration away from HTTP/SSE to CLI/stdio as the default transport.

_Last updated: 2025-05-13 🐹_
