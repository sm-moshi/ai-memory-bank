# Technology Stack

> Real technology choices and implementation details for AI Memory Extension

## 🏗 Core Runtime Stack

### **JavaScript Runtime**

- **Node.js**: 20.19.0+ (LTS) - Required for extension and MCP server
- **Package Manager**: pnpm 10.11.1 - Fast, efficient dependency management
- **Package Manager Config**: Workspace support, overrides for security patches

### **TypeScript Configuration**

- **TypeScript**: 5.8.3 - Latest with strict mode enabled
- **Config Structure**: Multi-config setup
  - `tsconfig.base.json` - Shared configuration
  - Root `tsconfig.json` - Main project configuration
  - `src/webview/tsconfig.json` - Webview-specific configuration
  - `src/webview/tsconfig.node.json` - Webview build tools
- **Features**: Project references, verbatimModuleSyntax, path mapping
- **Target**: ES2022 for modern Node.js compatibility

## 🔧 Build System & Tooling

### **Primary Build Pipeline**

- **Bundler**: Rollup 4.41+ - Modern bundler with excellent tree-shaking
- **Compiler**: SWC via @rollup/plugin-swc - 20x faster than TypeScript compiler
- **Build Performance**: Extension build ~300ms, Webview build ~817ms
- **Bundle Size**: Extension 405KB (106KB gzipped), optimized vendor chunking
- **Target Outputs**:
  - `dist/extension.cjs` - VS Code extension (CommonJS)
  - `dist/index.cjs` - MCP CLI server (CommonJS)
  - `dist/webview/` - React UI (ES modules)

### **Code Quality Tools**

- **Linting/Formatting**: Biome 1.9.4 - 100x faster than ESLint+Prettier
- **Performance**: 123 files linted in 57ms, formatting in 15ms
- **Configuration**: Proper exclusions for tsconfig*.json files (JSONC support)
- **Special Rules**: Interface naming (PascalCase), British English enforcement
- **Architecture**: Pure TypeScript source, no .js artifacts in git

### **Development Tools**

- **Path Aliases**: @/ and @utils/ for clean imports
- **Source Maps**: Enabled for development, optimized for production
- **Hot Reload**: Webview development with sub-100ms updates
- **Bundle Analysis**: Optional analysis with rollup plugins
- **Build Process**: Clean output, no warnings, automated artifact cleanup
- **File Management**: Intelligent exclusion of build artifacts from source control

## 🎨 Frontend Technology Stack

### **React Webview Implementation**

- **React**: 19.1.0 - Latest with concurrent features and React Compiler
- **Build Tool**: Vite 6.3.5 - Lightning-fast development server
- **UI Framework**: Tailwind CSS 4.1.8 - Native @tailwindcss/vite integration
- **VS Code Integration**: @vscode-elements/react-elements 1.15.1
- **Icons**: react-icons 5.5.0 - Comprehensive icon library

### **Webview Configuration**

- **Security**: Strict Content Security Policy (CSP)
- **Communication**: PostMessage for extension ↔ webview
- **Development**: Hot Module Replacement (HMR) with instant updates
- **Build Output**: Optimized chunks, vendor splitting, asset optimization

### **Package Management (Webview)**

- **Separate Workspace**: Independent pnpm workspace for webview
- **Dependencies**: React, Tailwind, VS Code Elements, React Icons
- **Dev Dependencies**: Vite, testing tools, TypeScript, Biome

## 🔌 Backend & Integration Stack

### **VS Code Extension Platform**

- **Engine**: VS Code 1.96.0+ - Extension host compatibility
- **Activation**: Specific activation events (not *)
- **Commands**: 6 registered commands with proper lifecycle
- **Configuration**: Extension settings via package.json contributes

### **MCP Server Implementation**

- **Protocol**: Model Context Protocol (MCP) SDK 1.12.1
- **Transport**: STDIO only (no HTTP) for maximum Cursor compatibility
- **Architecture**: Isolated child process with proper lifecycle management
- **Tools**: 7 MCP tools for memory bank operations

### **File System & Data Layer**

- **File Operations**: Node.js fs/promises with validation
- **Validation**: Zod 3.25.53 for runtime type checking
- **Caching**: LRU cache with automatic eviction
- **Streaming**: Intelligent file reading based on size thresholds
- **Templates**: gray-matter 4.0.3 for markdown frontmatter parsing

## 🧪 Testing Infrastructure

### **Testing Framework**

- **Test Runner**: Vitest 3.2.2 - 10x faster than Jest
- **Coverage**: @vitest/coverage-v8 with >90% threshold
- **Environment**: Node.js for extension, jsdom for webview

### **Testing Libraries**

- **React Testing**: @testing-library/react 16.3.0
- **DOM Testing**: @testing-library/dom 10.4.0, @testing-library/jest-dom 6.6.3
- **API Mocking**: MSW 2.9.0 with modern HttpResponse patterns
- **MCP Testing**: mcp-testing-kit 0.2.0 for direct server testing
- **Extension Testing**: @vscode/test-cli 0.0.11, @vscode/test-electron 2.5.2

### **Test Configuration**

- **Parallel Execution**: Multi-threaded test runs
- **Setup Files**: Global test configuration and MSW setup
- **Coverage Thresholds**: 90% lines, functions, branches, statements
- **Mock Patterns**: Centralized utilities for VS Code, filesystem, MCP mocking

## 🛠 Development Infrastructure

### **Build Scripts & Automation**

- **Development**: `pnpm dev` - Parallel watch with hot reload
- **Build**: `pnpm build` - Optimized production builds
- **Testing**: `pnpm test` - Full test suite with coverage
- **Quality**: `pnpm check` - Type checking, linting, formatting
- **Packaging**: `pnpm package` - VSIX extension packaging

### **Development Dependencies**

- **TypeScript**: @typescript/native-preview 7.0.0-dev for latest features
- **VS Code Tools**: @vscode/vsce 3.5.0 for extension packaging
- **Cross-Platform**: cross-env 7.0.3 for environment variables
- **Task Runner**: npm-run-all 4.1.5 for parallel script execution
- **Build Copy**: rollup-plugin-copy 3.5.0 for asset handling

## 🔐 Security & Performance

### **Security Tools**

- **Input Validation**: Zod schemas for all user inputs
- **Path Security**: Path traversal prevention
- **CSP**: Strict Content Security Policy for webview
- **Error Handling**: Secure error messages, no data leakage

### **Performance Characteristics**

- **Bundle Size**: Extension 405KB (106KB gzipped), webview <1MB initial
- **Build Speed**: ~600ms total build time
- **Linting Speed**: 123 files in 57ms with Biome
- **Memory Usage**: <50MB baseline, <100MB peak during development
- **File Operations**: Streaming for files >30KB, intelligent caching
- **Architecture**: Pure TypeScript source, zero build artifacts in git

## 📦 Dependencies Overview

### **Runtime Dependencies (3 total)**

- `@modelcontextprotocol/sdk`: MCP protocol implementation
- `gray-matter`: Markdown frontmatter parsing
- `zod`: Runtime type validation

### **Build Dependencies (Selective)**

- Core build tools: Rollup, SWC, Biome
- VS Code integration: @vscode/test-cli, @vscode/vsce
- Testing: Vitest, MSW, testing libraries
- Development: TypeScript, cross-env, npm-run-all

---

> Last updated: 6 June 2025
