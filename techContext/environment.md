# Environment

> Development environment setup and requirements for AI Memory Extension

## 🎯 Platform Requirements

### **Host Platform**

- **Operating Systems**: macOS, Linux, Windows (cross-platform support)
- **Development OS**: macOS (primary), Linux (CI/CD)
- **Architecture**: x64, arm64 (Apple Silicon support)
- **Current Environment**: macOS 14.5.0 (darwin 24.5.0)

### **Engine Requirements**

- **VS Code/Cursor**: ^1.96.2 (latest stable)
- **Node.js**: >=20.19.0 (LTS)
- **pnpm**: >=10.11.1 (latest with workspace support)
- **Git**: Required for version control and submodule management

## 🛠 Development Environment Setup

### **Prerequisites Installation**

```bash
# Check prerequisites
node --version    # Should be 20.19.0+
pnpm --version   # Should be 10.11.1+
code --version   # Should be 1.96.2+ (or cursor)

# Install Node.js 20 LTS if needed
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20
nvm use 20

# Install pnpm if needed
npm install -g pnpm@10.11.1
```

### **Initial Project Setup**

```bash
# Clone and setup project
git clone https://github.com/sm-moshi/aimemory.git
cd aimemory
pnpm install     # Install all dependencies
pnpm build       # Build extension and webview
pnpm test        # Verify setup with test suite
```

### **Package Manager Configuration**

- **Module Type**: ESM modules (`"type": "module"`)
- **Package Manager**: pnpm@10.11.1 with workspace support
- **Workspace Structure**: Main project + webview as separate workspace
- **Build Dependencies**: Limited to essential native modules for security:
  - `@biomejs/biome`, `@swc/core`, `@vscode/vsce-sign`, `esbuild`, `keytar`, `msw`
- **Package Extensions**: React peer dependency configuration

## 🔧 IDE Configuration

### **Cursor/VS Code Settings**

**Workspace Configuration** (`.vscode/settings.json`):

```json
{
  "editor.defaultFormatter": "biomejs.biome",
  "editor.formatOnSave": true,
  "typescript.preferences.importModuleSpecifier": "shortest",
  "biome.lspBin": "./node_modules/@biomejs/biome/bin/biome",
  "[typescript]": {
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
      "source.fixAll.biome": "explicit",
      "source.organizeImports.biome": "explicit"
    }
  },
  "typescript.format.enable": false,
  "typescript.validate.enable": true,
  "files.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.vscode-test": true
  }
}
```

### **Essential VS Code Extensions**

- **Biome** (biomejs.biome) - Official Biome extension for linting/formatting
- **TypeScript Importer** - Auto-import suggestions
- **GitLens** - Enhanced Git integration
- **Error Lens** - Inline error display
- **Auto Rename Tag** - HTML/JSX tag renaming
- **Thunder Client** (optional) - API testing during development

### **TypeScript Configuration**

- **Workspace uses strict mode** with project references
- **Path Mapping**: `@/` and `@utils/` aliases configured in tsconfig
- **Multi-config structure**: Base config + project-specific configs
- **Project references**: Root config references webview build config

## 🔄 Development Workflow

### **Daily Development Commands**

```bash
# Start development environment
pnpm dev              # Watch mode for extension + webview (parallel)

# Individual development modes
pnpm watch:extension  # Extension watch mode only
pnpm webview:dev      # Webview development server only

# Code quality checks (run before commits)
pnpm check           # TypeScript + linting + formatting
pnpm lint:fix        # Auto-fix linting issues
pnpm check-types     # TypeScript type checking only

# Testing during development
pnpm test            # Run full test suite
pnpm test:watch      # Continuous testing
pnpm test:coverage   # Generate coverage reports
```

### **Extension Development Workflow**

```bash
# Launch VS Code Extension Host for testing
# Method 1: Press F5 in Cursor/VS Code
# Method 2: Command Palette -> "Debug: Start Debugging"

# VSIX packaging for distribution
pnpm package         # Creates .vsix file in project root

# Development verification
pnpm health         # Quick health check (types + lint + test)
```

### **Hot Reload Setup**

- **Extension**: Automatic rebuild on source changes with Rollup watch mode
- **Webview**: Sub-100ms HMR updates with Vite development server
- **Parallel Development**: Both systems run simultaneously with `pnpm dev`

## 🌍 Environment Variables

### **Development Environment Variables**

```bash
# Optional environment variables for development
export NODE_ENV=development     # Development mode
export ANALYZE=true            # Enable bundle analysis
export VSCODE_TEST_DEBUG=1     # Debug extension tests
```

### **Production Build Environment**

```bash
export NODE_ENV=production     # Production optimizations
export VSCODE_TEST_TIMEOUT=30000  # Extended test timeout for CI
```

## 🌍 Cross-Platform Considerations

### **Path Handling**

- **Node.js**: `path.posix` for consistent cross-platform paths
- **File System**: Proper handling of case sensitivity differences
- **Line Endings**: Configured for LF throughout (biome.json)
- **Git Configuration**: Consistent line ending handling

### **Platform-Specific Setup**

**macOS**:

```bash
# No additional setup required
pnpm install && pnpm build
```

**Windows**:

```bash
# Ensure Git line ending configuration
git config --global core.autocrlf input
pnpm install && pnpm build
```

**Linux**:

```bash
# May need additional build tools
sudo apt-get install build-essential
pnpm install && pnpm build
```

## 🔧 Troubleshooting

### **Common Environment Issues**

**Node.js Version Mismatch**:

```bash
# Check current version
node --version
# Switch to required version
nvm use 20.19.0
```

**pnpm Installation Issues**:

```bash
# Clear pnpm cache
pnpm store prune
# Reinstall dependencies
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

**TypeScript/Biome Conflicts**:

- Ensure Biome is set as default formatter
- Disable TypeScript formatter: `"typescript.format.enable": false`
- Check for missing conditional statements before `throw`/`return`

**Extension Not Loading**:

- Verify VS Code version compatibility (^1.96.2)
- Check extension activation events in package.json
- Review output channel for error messages

**Build Failures**:

```bash
# Clean build
rm -rf dist
pnpm build:extension && pnpm build:webview
```

### **Performance Optimization**

- **Memory Usage**: Development environment should use <2GB
- **Build Speed**: Target ~600ms total build, <1s incremental
- **Test Speed**: Target <10s test suite execution
- **Hot Reload**: Should achieve sub-100ms webview updates

### **Development Tips**

- Use `pnpm health` for quick environment validation
- Enable Error Lens extension for inline TypeScript errors
- Use `pnpm perf:build` to monitor build performance
- Set up Git hooks with `pnpm check` for quality gates

---

> Last updated: 6 June 2025
