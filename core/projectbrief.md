# Project Brief

This is the internal, modular, and persistent memory bank for the AI Memory extension, enabling robust, context-aware AI workflows in Cursor and VS Code. It is designed for development, testing, and ongoing enhancement of the extension's memory features.

## Vision
- Enable reliable, context-driven AI assistance that persists across sessions and resets.
- Provide a modular memory bank structure for project, product, and technical context.
- Support both Cursor and VS Code environments, with a Cursor-first focus.

## Core Requirements
- Modular memory bank with subfolders: `core/`, `systemPatterns/`, `techContext/`, `progress/`.
- MCP server (CLI/stdio) for robust, context-driven file access and updates.
- Webview UI for memory bank management, including initialisation and updates.
- Self-healing logic to auto-create missing files from templates.
- Migration logic for flat → modular memory bank structures.
- Public documentation in `docs/`, private memory in `memory-bank/`.

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

_Last updated: 2025-05-18 🐹_
