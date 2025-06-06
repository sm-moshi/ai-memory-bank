# Project Brief

> AI Memory Extension for Cursor & VS Code - Production-ready memory bank system with MCP integration

## 🎯 Project Overview

**AI Memory** is a mature, production-ready extension that provides persistent, context-aware memory for LLMs and AI agents in Cursor and VS Code. The extension implements a robust memory bank system using the Model Context Protocol (MCP) for seamless AI integration.

**Current Version**: 0.8.0-dev.2
**Target Platforms**: Cursor 0.50+, VS Code 1.96+
**Architecture**: STDIO-based MCP server with React webview UI

## 🏗 Core Value Proposition

- **Zero Configuration**: Auto-creates and manages structured memory bank
- **High Performance**: Streaming operations, intelligent caching, async processing
- **Security Hardened**: Comprehensive input validation, path traversal protection
- **Self-Healing**: Auto-repair missing files, template-based recovery
- **Modern Stack**: React 19, Tailwind 4, TypeScript 5.8, Rollup+SWC

## 🎯 Primary Goals

### Phase 1 (✅ COMPLETE)

- [x] Core architecture with clean separation of concerns
- [x] MCP server implementation with STDIO transport
- [x] Modular memory bank structure (core/, systemPatterns/, techContext/, progress/)
- [x] Performance optimization with streaming and caching
- [x] Security hardening with Zod validation
- [x] React webview dashboard with VS Code Elements
- [x] Comprehensive testing (>90% coverage)
- [x] Modern build system (Rollup+SWC, Biome)

### Phase 2 (🔄 IN PROGRESS)

- [ ] AI metadata system for intelligent content organization
- [ ] Enhanced performance layer integration
- [ ] Advanced adapter infrastructure
- [ ] Improved codebase structure optimization

## 🏗 Architecture Principles

**Clean Architecture**: Utils → Services → Core → App layers
**Dependency Injection**: Testable, modular service design
**Result Pattern**: Explicit error handling throughout
**Event-Driven**: Async operations with proper cleanup
**Security First**: Input validation, path sanitization, resource limits

## 🎯 Target Users

**Primary**: Cursor users seeking persistent AI context
**Secondary**: VS Code users with AI workflows
**Developers**: Contributors to memory bank patterns

## 📊 Success Metrics

- **Adoption**: Extension installations and active users
- **Performance**: Sub-100ms operations, <50MB memory footprint
- **Reliability**: Zero data loss, graceful error recovery
- **Developer Experience**: <30s build times, hot reload webview

## 🛠 Technology Choices

**Core Runtime**: Node.js 20 LTS, TypeScript 5.8
**Build System**: Rollup 4 + SWC (20x faster than tsc)
**Code Quality**: Biome (100x faster than ESLint+Prettier)
**Frontend**: React 19, Tailwind 4, Vite 6
**Testing**: Vitest 3.2, MSW 2.9, @vscode/test-cli

---

> Last updated: 6 June 2025
