# Tech Context Index

> *Summary of technology stack, environment setup, and technical constraints*

## Architecture Overview

The AI Memory Extension is built with **Node.js 20 LTS** and **TypeScript**, leveraging **VS Code Extension API** and **MCP (Model Context Protocol)** for core functionality.

The webview interface is implemented with **React 19** and **Tailwind CSS 4** for a modern, responsive UI that integrates seamlessly with VS Code's design system.

## Modern Build System

Build and tooling systems use **Rollup** with **SWC** for 20x faster TypeScript compilation, **Biome** for unified linting and formatting, and **Vitest** for comprehensive testing with >90% coverage.

The build pipeline supports multiple targets: VS Code extension (CommonJS), MCP server (CommonJS), and webview (ES modules) with optimised bundles for each environment.

For complete build system specifications, see [stack.md](techContext/stack.md).
For detailed dependency list and versions, see [dependencies.md](techContext/dependencies.md).

## Quality Foundation

The project maintains strict TypeScript configuration with comprehensive error handling using the Result<T,E> pattern throughout. All inputs are validated with Zod schemas, and security is hardened with path sanitization and CSP implementation.

Performance is optimised through intelligent streaming for large files (>1MB) and bounded cache management with LRU eviction.

For current quality achievements and metrics, see [progress/current.md](progress/current.md).

## Technical Constraints & Design Principles

Key constraints include **Cursor MCP stdio transport compatibility**, **VS Code API limitations**, **performance requirements for large files**, and **comprehensive security validation**.

All dependencies and configurations are chosen for **rapid development velocity**, **long-term maintainability**, and **production reliability**.

## Runtime Environment

### Core Platform

* **Runtime**: Node.js 20 LTS (excellent ES2022 support and performance)
* **Operating System**: macOS 14.5+ primary, Linux/Windows compatibility
* **Shell**: Fish Shell (enhanced developer experience)
* **Package Manager**: pnpm 9.x (faster installs and better workspace support)

### Development Tools

* **Primary IDE**: Cursor with AI Memory Extension (self-hosting)
* **Target Platform**: VS Code API compatibility layer
* **Language**: TypeScript 5.x (strict mode with verbatimModuleSyntax)

For complete environment details, see [environment.md](techContext/environment.md).

## Technology Stack Highlights

### Backend Excellence

* **VS Code Extension API**: Native Cursor/VS Code integration with proper activation patterns
* **MCP Protocol**: stdio transport for reliable AI assistant communication
* **Zod Validation**: Runtime type safety and comprehensive input validation
* **Phase 1 → Phase 2**: Complete core architecture transitioning to metadata system

### Frontend Modernization

* **React 19**: Latest React with concurrent features and improved performance
* **Tailwind CSS 4**: Modern utility-first styling with enhanced performance
* **Vite**: Fast development server and optimised production builds
* **@vscode-elements/react-elements**: Native VS Code component integration

### Build & Quality Tools

* **Rollup + SWC**: 20x faster compilation compared to traditional TypeScript builds
* **Biome**: Unified linting and formatting replacing ESLint + Prettier
* **Vitest**: Modern testing framework with native ES module support
* **@vscode/test-cli**: Official extension testing with proper VS Code API mocking

For complete technology specifications, see [stack.md](techContext/stack.md).

## Development Status

The project follows a phased development approach targeting production readiness. Phase 1 (Core Architecture) completed ahead of schedule with all 181 tests passing and comprehensive performance optimizations operational.

For current development progress and roadmap, see [progress/current.md](progress/current.md).
For complete project goals and scope, see [core/projectbrief.md](core/projectbrief.md).

## Technical Achievements

### Performance Improvements

* **Streaming Optimization**: 75% memory usage reduction for large files through intelligent chunked reading
* **Build Performance**: 20x faster compilation with Rollup + SWC compared to traditional TypeScript
* **Test Performance**: Modern testing with Vitest providing 10x faster test execution

### Developer Experience

* **Type Safety Foundation**: Strict TypeScript with Result<T,E> pattern providing compile-time error prevention
* **Modern Toolchain**: Unified build system supporting extension, MCP server, and webview targets
* **Quality Automation**: Biome providing instant feedback with 100-character line width and tab indentation

### Standards Compliance

* **MCP Protocol**: Full stdio transport compatibility with Cursor's implementation
* **VS Code Integration**: Proper extension activation, commands, and webview security (CSP)
* **Security Standards**: Comprehensive input validation, path sanitization, and vulnerability prevention
* **British English**: Consistent language usage throughout codebase and documentation

---

The tech stack is designed to support rapid AI-assisted development while maintaining production-grade performance, security, and maintainability. The combination of modern tooling and strict quality standards provides a solid foundation for advanced memory management features.

---

> *Last updated: 2025-05-31*
