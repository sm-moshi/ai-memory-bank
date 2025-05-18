# Active Context

## Current Work Focus
- Integrate new MCP prompt constants (health check, file missing, update confirmation, structure guide, usage tip) into extension and webview actions for improved agent and user feedback.
- Continue refactor to remove Express and use Cursor/VS Code APIs for all communication (in progress).
- Ensure all MCP tool logic is Cursor-first (Cursor compatibility is the top priority).
- Enhance webview with advanced error/status feedback and file preview/diff features.
- Prepare for version control integration for memory bank files.

## Recent Changes
- Modularised memory bank structure (`core/`, `systemPatterns/`, `techContext/`, `progress/`).
- Added robust MCP server with error handling and port failover.
- Implemented webview UI for memory bank management (init, update, feedback).
- Refactored build and task automation (Justfile, scripts).
- Public documentation moved to `docs/`, private memory in `memory-bank/`.

## Next Steps
- Complete removal of Express and finalise Cursor-first refactor.
- Implement version control integration for memory bank files.
- Add visualisation tools for memory relationships in the webview.
- Gather user feedback for v0.2.x and plan for v1.0.0.
- Expand automated test coverage and CI/CD.

## Active Decisions and Considerations
- Prioritise Cursor compatibility; VS Code compatibility is a bonus.
- Plan for version control and remote/cloud support in future releases.
- Use Git Flow for release management.
- Maintain robust error handling and clear user feedback throughout development.

_Last updated: 2025-05-18_
