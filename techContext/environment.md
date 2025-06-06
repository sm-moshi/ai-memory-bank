# Environment

> Development environment setup and requirements for AI Memory Extension

## 🎯 Platform Requirements

### **Host Platform**

- **Operating Systems**: macOS, Linux, Windows (cross-platform support)
- **Development OS**: macOS (primary), Linux (CI/CD)
- **Architecture**: x64, arm64 (Apple Silicon support)
- **Current Environment**: macOS 14.5.0 (darwin 24.5.0)

### **Runtime Requirements**

- **Node.js**: 20.19.0+ LTS (specified in package.json engines)
- **Package Manager**: pnpm 10.11.1+ (specified in packageManager field)
- **VS Code/Cursor**: 1.96.2+ (vscode engine requirement)
- **Git**: Required for version control and submodule management

## 🛠 Development Environment Setup

### **Initial Setup**

```bash
# Prerequisites check
node --version    # Should be 20.19.0+
pnpm --version   # Should be 10.11.1+

# Project setup
git clone https://github.com/sm-moshi/aimemory.git
cd aimemory
pnpm install     # Install all dependencies
pnpm build       # Build extension and webview
```

### **Development Workflow**

```bash
# Development with hot reload
pnpm dev         # Parallel watch: extension + webview

# Testing during development
pnpm test        # Run full test suite
pnpm test:coverage  # Generate coverage report

# Code quality checks
pnpm check       # TypeScript + linting + formatting
pnpm lint:fix    # Auto-fix linting issues
```

### **Extension Development**

```bash
# Launch VS Code Extension Host
# Press F5 in Cursor to start extension development
# Or use Command Palette: "Debug: Start Debugging"

# VSIX packaging for distribution
pnpm package     # Creates .vsix file in project root
```

## 🔧 IDE Configuration

### **Cursor/VS Code Settings**

- **TypeScript**: Workspace uses strict mode with project references
- **Path Mapping**: @/ and @utils/ aliases configured in tsconfig
- **Formatters**: Biome formatter configured as default
- **Extensions**: Recommended extensions for optimal development experience

### **Recommended VS Code Extensions**

- **Biome**: Official Biome extension for linting/formatting
- **TypeScript Importer**: Auto-import suggestions
- **GitLens**: Enhanced Git integration
- **Error Lens**: Inline error display
- **Auto Rename Tag**: HTML/JSX tag renaming

### **Workspace Configuration**

```json
{
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnSave": true,
  "typescript.preferences.importModuleSpecifier": "shortest",
  "files.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.vscode-test": true
  }
}
```

## 📁 Project Structure & Paths

### **Source Directory Organization**

```md
src/
├── core/              # Business logic & services
├── mcp/              # MCP server implementation
├── webview/          # React UI (separate pnpm workspace)
├── types/            # TypeScript definitions
├── utils/            # Utility functions
├── app/              # Application layer
├── cursor/           # Cursor integration
├── performance/      # Performance optimizations
└── test/             # Test utilities and setup
```

### **Build Output Structure**

```md
dist/
├── extension.cjs     # Main VS Code extension
├── index.cjs         # MCP CLI server
├── webview/          # React UI build output
└── assets/           # Extension assets (icons, etc.)
```

### **Configuration Files**

- **tsconfig.base.json**: Shared TypeScript configuration
- **tsconfig.json**: Main project TypeScript config
- **rollup.config.js**: Extension build configuration
- **biome.json**: Code quality configuration
- **vitest.config.ts**: Test configuration

## 🔄 Development Scripts

### **Primary Development Commands**

```bash
# Development
pnpm dev              # Watch mode for extension + webview
pnpm webview:dev      # Webview development server only

# Building
pnpm build            # Production build (extension + webview)
pnpm build:extension  # Extension only
pnpm build:webview    # Webview only

# Testing
pnpm test             # Run all tests
pnpm test:unit        # Unit tests only
pnpm test:extension   # VS Code extension tests
pnpm test:coverage    # Generate coverage reports

# Quality
pnpm check            # Type check + lint + format
pnpm lint:fix         # Auto-fix linting issues
pnpm check-types      # TypeScript type checking only
```

### **Specialized Commands**

```bash
# Package management
pnpm package          # Create VSIX extension package
pnpm update-docs-date # Update documentation timestamps

# Development tools
pnpm build-check      # Validate build output
pnpm refactor-imports # Clean up import statements
```

## 🧪 Testing Environment

### **Test Configuration**

- **Framework**: Vitest 3.2.2 with V8 coverage
- **Environment**: Node.js for extension, jsdom for webview
- **Coverage Threshold**: >90% lines, functions, branches, statements
- **Parallel Execution**: Multi-threaded test runs

### **Test Data & Mocks**

- **Centralized Utilities**: `src/test/test-utils/` for shared mocks
- **VS Code Mocking**: Complete VS Code API mock implementations
- **MCP Testing**: mcp-testing-kit for direct server testing
- **API Mocking**: MSW 2.9.0 for HTTP request mocking

### **CI/CD Environment**

- **GitHub Actions**: Automated testing on push/PR
- **Cross-Platform**: Test on macOS, Linux, Windows
- **Node.js Matrix**: Test multiple Node.js versions
- **Coverage Reports**: Automated coverage tracking

## 🔒 Security Environment

### **Development Security**

- **Input Validation**: Zod schemas for all user inputs
- **Path Security**: Workspace boundary enforcement
- **CSP**: Strict Content Security Policy for webview
- **Secret Management**: No hardcoded secrets, .secrets file gitignored

### **Build Security**

- **Dependency Auditing**: Regular pnpm audit runs
- **VSIX Scanning**: Extension package security validation
- **Code Analysis**: Biome security rules enabled
- **License Compliance**: All dependencies use permissive licenses

## 📊 Performance Environment

### **Development Performance**

- **Build Speed**: ~600ms total build, <1s incremental with SWC
- **Linting Speed**: 123 files in 57ms with Biome
- **Test Speed**: <10s test suite execution with Vitest
- **Hot Reload**: Sub-100ms webview updates with Vite HMR
- **Memory Usage**: <2GB development environment
- **Architecture**: Pure TypeScript source, zero build artifacts

### **Runtime Performance Targets**

- **Extension Activation**: <1s startup time
- **Memory Footprint**: <50MB baseline, <100MB peak
- **File Operations**: <100ms for normal operations
- **Webview Loading**: <500ms initial load

## 🌍 Cross-Platform Considerations

### **Path Handling**

- **Node.js**: path.posix for consistent cross-platform paths
- **File System**: Proper handling of case sensitivity differences
- **Line Endings**: Configured for LF throughout (biome.json)

### **Platform-Specific Tools**

- **Package Manager**: pnpm works consistently across platforms
- **Build Tools**: Rollup + SWC cross-platform compatibility
- **Testing**: Vitest runs consistently on all platforms
- **Git**: Submodule support for memory-bank structure

---

> Last updated: 6 June 2025
