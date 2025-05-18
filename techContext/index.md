# Tech Context Index

## Technologies Used

- TypeScript, Node.js
- React (webview UI)
- Tailwind CSS (webview styling)
- Vite (webview build system)
- Model Context Protocol (MCP) SDK
- zod (schema validation)
- express, cors (to be refactored out)

## Development Setup

- Install dependencies with npm or pnpm.
- Use MCP tools for memory bank management (`initialize-memory-bank`, `list-memory-bank-files`, `get-memory-bank-file`, `update-memory-bank-file`).
- Webview dashboard for user actions (initialise, update, feedback).
- All file operations are async and robust, with readiness checks and error handling.
- Modular file structure enforced for all memory bank operations.

## Technical Constraints

- Must support both Cursor and VS Code (Cursor-first priority).
- Robust error handling and feedback required for all user actions.
- Modular file structure enforced for all memory bank operations.
- Migration logic for flat → modular memory bank structures.
- Public documentation in `docs/`, private memory in `memory-bank/`.

## Dependencies

- @modelcontextprotocol/sdk
- zod
- express, cors (to be refactored out)

## Experimental/Advanced Features

- Chunked file access, metadata, and planner tools (see `EXPERIMENTAL-MCP-PLAN.md`).
- Version control integration, remote/cloud support, and visualisation tools are in planning.

_Last updated: 2025-05-09_
