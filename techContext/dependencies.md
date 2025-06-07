# Dependencies

> Dependency analysis and management for AI Memory Extension

## 📦 Production Dependencies (3 total)

### **@modelcontextprotocol/sdk** (^1.12.1)

- **Purpose**: Core MCP protocol implementation for Cursor integration
- **Usage**: MCP server, tool registration, STDIO transport
- **Critical**: ✅ Essential for core functionality
- **Size**: ~500KB | **License**: Open source, permissive

### **zod** (^3.25.56)

- **Purpose**: Runtime type validation and schema parsing
- **Usage**: Input validation, MCP tool parameters, security boundaries
- **Critical**: ✅ Essential for security and type safety
- **Size**: ~50KB | **License**: MIT

### **gray-matter** (^4.0.3)

- **Purpose**: Markdown frontmatter parsing for templates
- **Usage**: Template processing, metadata extraction
- **Critical**: 🔶 Important for template system
- **Size**: ~25KB | **License**: MIT

## 🛠 Development Dependencies

### **Build System**

- **rollup** (^4.42.0) + plugins - Modern bundling with tree-shaking
- **@swc/core** (^1.11.31) - Rust-based TypeScript compilation
- **@biomejs/biome** (^1.9.4) - Unified linting and formatting
- **typescript** (^5.8.3) + **@typescript/native-preview** (^7.0.0-dev.20250606.1)

### **Testing Infrastructure**

- **vitest** (^3.2.2) + **@vitest/coverage-v8** (^3.2.2) - Modern test runner
- **msw** (^2.10.0) - API mocking with HttpResponse patterns
- **mcp-testing-kit** (^0.2.0) - Specialized MCP server testing
- **@testing-library/react** (^16.3.0) - Component testing best practices

### **Webview Frontend**

- **react** (^19.1.0) + **react-dom** (^19.1.0) - UI framework
- **tailwindcss** (^4.1.8) + **@tailwindcss/vite** (^4.1.8) - CSS framework
- **@vscode-elements/react-elements** (^1.15.1) - VS Code native theming
- **vite** (^6.3.5) + **@vitejs/plugin-react** (^4.5.1) - Build tooling

### **VS Code Integration**

- **@vscode/test-cli** (^0.0.11) + **@vscode/test-electron** (^2.5.2) - Extension testing
- **@vscode/vsce** (^3.5.0) - Extension packaging
- **@types/vscode** (1.96.0) - VS Code API types

### **Utilities**

- **commander** (^14.0.0) - CLI parsing
- **cross-env** (^7.0.3) - Environment variables
- **npm-run-all** (^4.1.5) - Parallel script execution

## 🔒 Security & Quality

### **Package Management Security**

- **pnpm Configuration**: `onlyBuiltDependencies` limited to essential native modules
- **Audit Process**: Regular `pnpm audit` with immediate security patches
- **Version Strategy**: Exact versions for build tools, caret ranges for libraries

### **Bundle Monitoring**

- **Production Targets**: <3MB extension, <1MB webview initial
- **Optimization**: Tree-shaking, vendor chunking, dynamic imports
- **Analysis**: Optional bundle analysis with rollup-plugin-analyzer

## 📈 Dependency Strategy

### **Minimalist Philosophy**

- **Production**: Only 3 runtime dependencies (excellent security posture)
- **Principle**: Prefer Node.js/browser built-ins over external libraries
- **Quality**: Each dependency serves critical, irreplaceable functionality
- **Licenses**: All MIT/permissive, no GPL or restrictive licenses

### **Version Management**

- **Build Tools**: Exact versions for reproducible builds
- **Libraries**: Caret ranges for security patches
- **Breaking Changes**: Comprehensive testing before major updates
- **Security**: Automated vulnerability scanning and prompt patching

### **Performance Impact**

- **Bundle Analysis**: Track dependency impact on final bundle size
- **Tree Shaking**: Verify unused code elimination effectiveness
- **Load Time**: Monitor first-load performance impact
- **Memory Usage**: Development dependencies don't affect runtime memory

## 🚨 Risk Assessment

### **Low Risk Profile**

✅ **Minimal Attack Surface**: Only 3 production dependencies
✅ **Mature Ecosystem**: React, TypeScript, Node.js proven stability
✅ **Active Maintenance**: All dependencies actively developed and patched
✅ **License Safety**: MIT/permissive licenses throughout

### **Monitoring Strategy**

- **Weekly**: Automated security vulnerability scanning
- **Monthly**: Dependency version review and strategic updates
- **Quarterly**: Major version evaluation and upgrade planning
- **Emergency**: Immediate security patches with comprehensive testing

---

> Last updated: 6 June 2025
