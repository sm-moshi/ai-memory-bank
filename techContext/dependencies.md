# Dependencies

> Dependency analysis and management optimized for AI Memory Extension **Last Updated**: 2025-06-07

---

## 📦 **Production Dependencies**

### **Core Runtime Requirements**

```json
{
  "@modelcontextprotocol/sdk": "^1.12.1",    // MCP protocol implementation
  "commander": "^14.0.0",                    // CLI argument parsing
  "gray-matter": "^4.0.3",                   // Markdown frontmatter parsing
  "zod": "^3.25.56"                          // Runtime type validation
}
```

**Rationale**: Minimal runtime footprint with essential functionality only.

---

## 🛠️ **Development Dependencies**

### **Build System** (Modern & Fast)

```json
{
  "@rollup/plugin-alias": "^5.1.1",         // Path resolution
  "@rollup/plugin-commonjs": "^28.0.3",     // CJS compatibility
  "@rollup/plugin-json": "^6.1.0",          // JSON imports
  "@rollup/plugin-node-resolve": "^16.0.1", // Node module resolution
  "@rollup/plugin-swc": "^0.4.0",           // SWC TypeScript compilation
  "@swc/core": "^1.11.31",                  // Fast TypeScript compiler
  "rollup": "^4.42.0",                      // Extension bundler
  "rollup-plugin-analyzer": "^4.0.0",       // Bundle analysis
  "rollup-plugin-copy": "^3.5.0"            // Asset copying
}
```

### **TypeScript Tooling** (Cutting Edge)

```json
{
  "@typescript/native-preview": "^7.0.0-dev.20250607.1", // tsgo - 10x faster
  "typescript": "^5.8.3",                   // TypeScript compiler fallback
  "vite-tsconfig-paths": "^5.1.4"          // Path mapping for Vite
}
```

### **Code Quality** (Unified Tooling)

```json
{
  "@biomejs/biome": "^1.9.4"               // Unified linting + formatting
}
```

### **Testing Framework** (Modern Vitest)

```json
{
  "@vitest/coverage-v8": "^3.2.2",         // Code coverage
  "vitest": "^3.2.2"                       // Testing framework
}
```

### **VS Code Integration**

```json
{
  "@types/node": "^20.19.0",               // Node.js types
  "@types/vscode": "1.96.0",               // VS Code API types
  "@vscode/test-cli": "^0.0.11",           // Extension testing
  "@vscode/vsce": "^3.5.0"                 // Extension packaging
}
```

### **Utility Tools**

```json
{
  "@types/glob": "^8.1.0",                 // Glob pattern types
  "glob": "^11.0.2",                       // File pattern matching
  "tsx": "^4.19.4"                         // TypeScript script execution
}
```

---

## 🔧 **pnpm Workspace Configuration**

### **✅ Optimized Build Script Management**

```json
{
  "pnpm": {
    "onlyBuiltDependencies": [
      "@biomejs/biome",     // Code quality tooling
      "@swc/core",          // Fast compilation
      "@tailwindcss/oxide", // CSS engine
      "@vscode/vsce-sign",  // Extension signing
      "esbuild",            // Transitive via Vite
      "keytar"              // Secure credential storage
    ]
  }
}
```

**Benefits**:

- ✅ **Zero build warnings** during installation
- ✅ **Automated approval** for trusted packages
- ✅ **Security boundary** for unknown packages
- ✅ **Workspace inheritance** works from any directory

### **Dependency Approval Strategy**

1. **Pre-approved** via `onlyBuiltDependencies` (common tools)
2. **One-time approval** for transitive dependencies (msw, etc.)
3. **Manual review** for new/unknown packages
4. **CI/CD compatible** - no interactive prompts

---

## 📊 **Webview Dependencies**

### **React 19 Modern Stack**

```json
{
  "@tailwindcss/vite": "^4.1.8",
  "@vscode-elements/elements": "^1.16.1",
  "@vscode-elements/react-elements": "^1.15.1",
  "react": "^19.1.0",
  "react-dom": "^19.1.0",
  "react-icons": "^5.5.0"
}
```

### **Development Tooling**

```json
{
  "@vitejs/plugin-react": "^4.5.1",
  "babel-plugin-react-compiler": "^19.1.0-rc.2",
  "tailwindcss": "^4.1.8",
  "vite": "^6.3.5"
}
```

**Note**: Webview inherits workspace pnpm configuration from root.

---

## 🚀 **Performance Optimizations**

### **Fast TypeScript Compilation**

- **Primary**: `tsgo` via `@typescript/native-preview` (10x faster)
- **Fallback**: Standard `tsc` for unsupported features
- **SWC Plugin**: 20x faster than standard TypeScript plugin

### **Bundle Optimization**

- **Tree-shaking**: ES2022 imports for modern bundling
- **Source maps**: Development only for faster builds
- **Bundle analysis**: On-demand with `ANALYZE=true`

### **Caching Strategy**

- **pnpm**: Shared dependency cache across workspace
- **Rollup**: Efficient incremental builds
- **Vite**: Sub-100ms HMR for webview development

---

## 🔍 **Dependency Analysis Tools**

### **Inspection Commands**

```bash
pnpm why [package]           # Trace dependency origin
pnpm list --depth=0          # Show direct dependencies
pnpm outdated               # Check for updates
pnpm audit                  # Security vulnerability scan
```

### **Build Script Management**

```bash
pnpm approve-builds         # Approve specific packages
pnpm install               # Clean install (zero warnings)
```

---

## 📋 **Maintenance Guidelines**

### **Adding New Dependencies**

1. **Runtime dependencies** → `dependencies` in appropriate package.json
2. **Build tools** → `devDependencies` in root package.json
3. **Webview-specific** → `devDependencies` in webview package.json
4. **Build scripts** → Add to `onlyBuiltDependencies` if trusted

### **Version Updates**

1. **Security patches** → Apply immediately
2. **Minor updates** → Test in development first
3. **Major updates** → Coordinate with build system changes
4. **LTS alignment** → Prefer Node.js LTS-compatible versions

### **Quality Gates**

- ✅ **Zero build warnings** for clean installations
- ✅ **Consistent versioning** across workspace
- ✅ **Security compliance** via automated audits
- ✅ **Performance validation** for build tool changes

---

## 🎯 **Success Metrics**

- **Installation time**: < 30 seconds for fresh workspace
- **Build warnings**: Zero for normal development workflow
- **Security score**: No high/critical vulnerabilities
- **Bundle size**: < 2MB for extension package
- **Startup time**: < 500ms for extension activation

---

*Optimized for modern development experience with zero-friction dependency management.*
