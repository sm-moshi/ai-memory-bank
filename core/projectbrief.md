# Project Brief

The AI Memory extension is designed to empower Cursor and VS Code users with a persistent, modular, and user-editable memory bank. This enables context-aware AI interactions, seamless context resets, and a more reliable development workflow.

## Vision
- Enable robust, context-driven AI assistance that persists across sessions and resets.
- Provide a modular memory bank structure for project, product, and technical context.
- Support both Cursor and VS Code environments for maximum flexibility.

## Core Requirements
- Modular memory bank with subfolders: `core/`, `systemPatterns/`, `techContext/`, `progress/`.
- MCP server for robust, context-driven file access and updates.
- Webview UI for memory bank management, including initialisation and updates.
- Compatibility with Cursor (0.49+) and VS Code.
- Migration logic for flat → modular memory bank structures.
- Public documentation in `docs/`, private memory in `memory-bank/`.
- **MCP CLI/stdio entrypoint for Cursor 0.50+ compatibility, enabling context-agnostic operation and robust integration.**

## Project Goals
- Provide tools for initialisation, status, and updates of the memory bank.
- Support migration from flat to modular memory bank structures.
- Enable future enhancements such as version control, remote/cloud support, and visualisation tools.
- Ensure robust error handling, clear feedback, and user-centric design.

## Project Scope
- Core features: modular memory bank, MCP server, webview UI, migration logic.
- Planned features: version control integration, enhanced webview (file previews, diffs, history), remote/cloud support, visualisation tools, improved AI context workflows.
- Long-term: AI-driven memory bank suggestions, multi-project management, plugin system for custom modules.

## Best Practices
- Use the webview for all memory bank actions.
- Keep extension and dependencies up to date.
- Review feedback and logs for errors.
- Follow Git Flow for release management.

_Last updated: 2025-05-13_
