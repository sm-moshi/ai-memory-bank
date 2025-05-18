# Product Context

This is the internal, modular, and persistent memory bank for the AI Memory extension, designed to enable robust, context-aware AI workflows in Cursor and VS Code. It is structured for development, testing, and ongoing enhancement of the extension's memory features.

## Why This Project Exists
- Developers using AI tools in Cursor/VS Code need reliable, persistent, and modular project memory to avoid context loss during resets or workspace reloads.
- Manual context management is error-prone and inefficient; a standard, user-friendly solution is required for both development and production.

## Problems It Solves
- Loss of context during AI or workspace resets.
- Lack of modular, user-editable project memory for development and testing.
- Manual, error-prone context management.
- Difficulty in maintaining project continuity and knowledge across sessions.

## How It Works
- The extension initialises a modular memory bank structure (`core/`, `systemPatterns/`, `techContext/`, `progress/`) for context-aware AI interactions.
- Self-healing logic ensures all required files are present, auto-creating any missing ones from templates.
- Users manage memory bank files via the webview dashboard or MCP commands, with clear feedback and robust error handling.
- The MCP server (CLI/stdio) provides robust, context-driven file access and updates.
- The design is Cursor-first, with VS Code compatibility as a bonus.

## User Experience Goals
- Simple installation and setup (via Cursor/VS Code extension panel or VSIX file).
- Intuitive webview dashboard for memory management.
- Clear feedback and robust error handling for all actions.
- Seamless integration with existing development workflows.
- Support for advanced features like version control and remote/cloud memory in future releases.

_Last updated: 2025-05-18 🐹_
