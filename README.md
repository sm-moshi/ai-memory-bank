# 🧠 AI Memory Bank (the internal development one)

This folder contains the modular, persistent memory bank for the AI Memory extension. It is designed to enable robust, context-aware AI workflows in Cursor and VS Code.

---

## 📦 Structure

The memory bank is organised into subfolders, each serving a specific context:

- `core/` — Project vision, product context, and active context
- `systemPatterns/` — Architecture, design patterns, and scanning strategies
- `techContext/` — Technology stack, dependencies, and environment
- `progress/` — Current progress, history, and project roadmap

Each subfolder contains Markdown files that are loaded, updated, and managed by the extension and MCP server.

---

## ✨ Key Features

- **Modular structure:** Keeps project, technical, and progress context separate and easy to manage.
- **Self-healing:** Missing or incomplete files are auto-created from templates.
- **MCP server integration:** Robust, context-driven file access and updates via CLI/stdio.
- **Webview UI:** Initialise, update, and repair the memory bank with clear feedback.
- **Migration logic:** Supports migration from flat to modular memory banks.
- **Cursor-first:** Designed for Cursor, with VS Code compatibility as a bonus.

---

## 🚦 Progress Tracking

- [Current Progress](./progress/current.md): Active work, status, known issues, and next steps.
- [Progress History](./progress/history.md): Completed work, milestones, and achievements.

Start with `progress/index.md` for an overview of the progress tracking system.

---

## 🏗 System Patterns & Architecture

- Service-oriented architecture for extension components (MemoryBankService, MCPServer, CommandHandler, WebviewManager).
- CLI/stdio transport for robust, context-agnostic integration with Cursor.
- Async, robust file operations with readiness checks and error handling.
- See [systemPatterns/index.md](./systemPatterns/index.md) for more.

---

## 🛠 Tech Context

This project uses a modular, strictly typed stack designed for robust AI memory management within Cursor and VS Code.

- The backend (extension and MCP server) is built with Node.js and TypeScript, leveraging the Model Context Protocol (MCP) SDK for memory bank operations.
- The frontend webview is implemented in React 19 with Tailwind CSS and Vite for a modern, responsive UI.
- Build and test systems use esbuild, Vite, Vitest, and ESLint, ensuring reliability and maintainability.
- Key constraints include strict type safety, modular file structure, Cursor-first compatibility, and comprehensive automated testing.
- All dependencies and configurations are chosen for clarity, maintainability, and seamless integration with the Cursor/VS Code ecosystem.

See [techContext/index.md](./techContext/index.md) for details on stack, dependencies, and environment.

---

## 📚 Best Practices

- Use the webview for all memory bank actions.
- Keep extension and dependencies up to date.
- Review feedback and logs for errors.
- Follow Git Flow for release management.

---

## 📝 Notes

- All files in this folder are managed by the extension and should not be edited manually unless you know what you're doing.
- For more information, see the main project [README](../README.md) and [docs](../docs/).

---

_Last updated: 2025-05-18 🐹_
