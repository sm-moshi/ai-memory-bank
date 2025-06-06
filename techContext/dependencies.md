# Dependencies

> Real dependency analysis and management for AI Memory Extension

## 📦 Production Dependencies (3 total)

### **@modelcontextprotocol/sdk** (^1.12.1)

- **Purpose**: Core MCP protocol implementation for Cursor integration
- **Usage**: MCP server, tool registration, STDIO transport
- **Critical**: ✅ Essential for core functionality
- **License**: Open source, permissive license
- **Size Impact**: ~500KB (reasonable for protocol implementation)

### **zod** (^3.25.53)

- **Purpose**: Runtime type validation and schema parsing
- **Usage**: Input validation, MCP tool parameters, file content validation
- **Critical**: ✅ Essential for security and type safety
- **License**: MIT
- **Size Impact**: ~50KB (excellent for comprehensive validation)

### **gray-matter** (^4.0.3)

- **Purpose**: Markdown frontmatter parsing for memory bank templates
- **Usage**: Template processing, metadata extraction from markdown files
- **Critical**: 🔶 Important for template system
- **License**: MIT
- **Size Impact**: ~25KB (lightweight, focused utility)

## 🛠 Development Dependencies (Strategic Selection)

### **Build System Core**

- **rollup** (^4.41.1) - Modern bundler with tree-shaking
- **@rollup/plugin-swc** (^0.4.0) - 20x faster TypeScript compilation
- **@swc/core** (^1.11.31) - Rust-based compiler for speed
- **vite** (^6.3.5) - Lightning-fast webview development

### **Code Quality**

- **@biomejs/biome** (^1.9.4) - 100x faster than ESLint+Prettier
- **typescript** (^5.8.3) - Latest TypeScript with strict mode
- **@typescript/native-preview** (7.0.0-dev) - Cutting-edge TypeScript features

### **Testing Infrastructure**

- **vitest** (^3.2.2) - 10x faster than Jest, native ESM
- **@vitest/coverage-v8** (^3.2.2) - V8 coverage for accurate metrics
- **msw** (^2.9.0) - Modern API mocking with HttpResponse patterns
- **mcp-testing-kit** (^0.2.0) - Specialized MCP server testing

### **React Testing (Webview)**

- **@testing-library/react** (^16.3.0) - Component testing best practices
- **@testing-library/dom** (^10.4.0) - DOM testing utilities
- **@testing-library/jest-dom** (^6.6.3) - Enhanced DOM assertions
- **jsdom** (^26.1.0) - Browser environment simulation

### **VS Code Integration**

- **@vscode/test-cli** (^0.0.11) - Official extension testing
- **@vscode/test-electron** (^2.5.2) - Electron-based test runner
- **@vscode/vsce** (^3.5.0) - Extension packaging and publishing

## 📊 Webview Dependencies (React UI)

### **Core React Stack**

- **react** (^19.1.0) - Latest with concurrent features
- **react-dom** (^19.1.0) - DOM renderer for React 19
- **@types/react** (^19.1.6) - TypeScript definitions
- **@types/react-dom** (^19.1.6) - DOM TypeScript definitions

### **UI Framework**

- **tailwindcss** (^4.1.8) - Utility-first CSS framework
- **@tailwindcss/vite** (^4.1.8) - Native Vite integration (no PostCSS!)
- **@vscode-elements/elements** (^1.16.1) - VS Code UI components
- **@vscode-elements/react-elements** (^1.15.1) - React wrappers
- **react-icons** (^5.5.0) - Comprehensive icon library

### **Development Tools (Webview)**

- **@vitejs/plugin-react** (^4.5.1) - React integration for Vite
- **vite-tsconfig-paths** (^5.1.4) - Path mapping support
- **@vitest/ui** (^3.2.2) - Visual test runner interface

## 🔒 Security & Quality Management

### **Package Management Security**

- **pnpm Overrides**: Configured for security patches
- **onlyBuiltDependencies**: Limited to essential native modules
- **Audit Process**: Regular dependency auditing with pnpm audit
- **Version Pinning**: Exact versions for build tools, ranges for libraries

### **Bundle Size Monitoring**

- **Production Bundle**: <3MB for extension, <1MB for webview
- **Tree Shaking**: Rollup eliminates unused code effectively
- **Dynamic Imports**: Code splitting for lazy loading
- **Vendor Chunking**: Separate chunks for React, VS Code Elements

## 📈 Dependency Strategy

### **Minimalist Approach**

- **Production**: Only 3 runtime dependencies (excellent!)
- **Principle**: Prefer built-in Node.js/browser APIs when possible
- **Quality over Quantity**: Each dependency serves critical functionality
- **License Compliance**: All MIT/permissive licenses

### **Version Management**

- **Build Tools**: Exact versions for reproducible builds
- **Libraries**: Caret ranges (^) for patch updates
- **Security**: Prompt updates for security vulnerabilities
- **Breaking Changes**: Careful evaluation of major version updates

### **Development Efficiency**

- **Modern Tools**: SWC, Biome, Vitest for 10x+ speed improvements
- **TypeScript**: Latest features with native preview builds
- **Hot Reload**: Instant development feedback with Vite
- **Testing**: Fast, reliable test suite with excellent DX

## 🚨 Dependency Risks & Mitigation

### **Low Risk Profile**

- **Small Surface Area**: Only 3 production dependencies
- **Mature Ecosystem**: React, TypeScript, Node.js stability
- **Active Maintenance**: All dependencies actively maintained
- **License Compatibility**: MIT/Apache licenses throughout

### **Monitoring & Updates**

- **Security Alerts**: GitHub dependabot enabled
- **Version Tracking**: Regular review of dependency updates
- **Breaking Changes**: Thorough testing before major updates
- **Fallback Plans**: Core functionality not dependent on any single library

## 📋 Dependency Audit Status

### **Latest Audit Results** ✅

- **Security Vulnerabilities**: 0 known vulnerabilities
- **Outdated Packages**: All dependencies current within acceptable ranges
- **Bundle Impact**: All dependencies justified by functionality value
- **License Review**: All licenses compatible with extension licensing
- **Build Performance**: Rollup + SWC delivering fast compilation
- **Code Quality**: Biome providing unified linting and formatting
- **Architecture**: Pure TypeScript source, zero build artifacts

### **Maintenance Schedule**

- **Weekly**: Security vulnerability scanning
- **Monthly**: Dependency version review and updates
- **Quarterly**: Major version evaluation and planning
- **As Needed**: Emergency security patches

---

> Last updated: 6 June 2025
