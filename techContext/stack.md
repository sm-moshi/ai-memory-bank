# Technology Stack

Technology choices and implementation decisions for AI Memory Extension

## Core Technology Decisions

### Build System & Compilation

#### TypeScript Compilation Strategy

- **Primary**: tsgo via `@typescript/native-preview` (TypeScript 7.0 native port)
- **Rationale**: 10x performance improvement over Node.js-based tsc
- **Implementation**: Go-based native compiler with shared memory parallelism
- **Fallback**: Standard tsc for unsupported features (--build, full declaration emit)
- **Editor Integration**: `"typescript.experimental.useTsgo": true` in VS Code/Cursor

#### tsgo Current Status (2025)

- ✅ Full type checking parity with TS 5.8
- ✅ JSX support and JavaScript+JSDoc checking
- ✅ Basic editor features (diagnostics, hover, completions)
- ⚠️ Limited: No --build mode, limited declaration emit, no incremental builds
- 🔄 In progress: Auto-imports, find-all-references, rename

#### Performance Characteristics

- **VS Code codebase**: 77s → 7.5s (10.4x improvement)
- **Playwright codebase**: 11s → 1.1s (10.1x improvement)
- **Editor load time**: 8x improvement for large projects
- **Memory efficiency**: Native compilation without Node.js overhead

#### Module Resolution Strategy

- **Recommended**: `node16`, `nodenext`, or `bundler`
- **Deprecated**: `node`/`node10` (causes errors with tsgo)
- **Project Structure**: Multi-config with project references
- **Path Mapping**: Clean import aliases with verbatimModuleSyntax

### Build Pipeline Architecture

#### Primary Build: Rollup + SWC

- **Rollup 4.42+**: Modern bundler with excellent tree-shaking
- **SWC Integration**: 20x faster TypeScript compilation vs tsc
- **Multi-Target Support**: Extension (CommonJS), MCP CLI (CommonJS), Webview (ES modules)
- **Optimization**: Dead code elimination, smart externalization, bundle splitting

#### Webview Build: Vite + React 19

- **Vite 6.3.5+**: Lightning-fast HMR with sub-100ms updates
- **React 19.1+**: Concurrent features, automatic batching, React Compiler
- **Tailwind CSS 4.1.8+**: Native CSS engine with container queries
- **Performance**: Native ES modules in development, optimized bundles for production

### Code Quality & Testing

#### Unified Tooling: Biome

- **Single Tool**: Replaces ESLint + Prettier for speed and consistency
- **Configuration**: Tabs (4 spaces), 100 char lines, double quotes
- **Parallel Processing**: Multi-threaded analysis for large codebases
- **Smart Import Organization**: Automatic sorting, grouping, unused removal

#### Testing Stack: Vitest + MSW

- **Vitest 3.2+**: Native ESM support with streaming test runner
- **MSW 2.10+**: Modern API mocking with http.get() and HttpResponse.json()
- **mcp-testing-kit 0.2.0**: Direct MCP server testing with dummy transport
- **Coverage**: >90% threshold with V8 provider, parallel execution

#### Biome 2.0 Beta Consideration (2025-06-07)

- **Biome 2.0 Beta** introduces:
  - Plugin support (GritQL-based custom lint rules)
  - Domains (framework/tech-specific rule groups)
  - Multi-file analysis (cross-file linting, e.g. import cycles)
  - Import organizer revamp (smarter, customizable sorting/merging)
  - Assists (actions without diagnostics, e.g. sort object keys)
  - Improved suppressions (file/range, e.g. // biome-ignore-all)
  - HTML formatter (experimental)
  - Monorepo support and config changes
- **Plan:**
  - Monitor Biome 2.0 Beta progress and stability
  - Evaluate migration when stable for plugin/domain/multi-file features
  - Target: Enhanced code quality, project-specific rules, and monorepo linting

### Package Management: pnpm

- **Workspace Configuration**: Monorepo support with webview as separate workspace
- **Security**: Overrides for security patches, onlyBuiltDependencies configuration
- **Performance**: supportedArchitectures for faster installs
- **Reproducibility**: Always commit pnpm-lock.yaml

## Technology Integration Patterns

### Development Workflow

- **Parallel Development**: Watch mode + webview dev server simultaneously
- **Quality Gates**: Type checking (tsgo) + linting (Biome) + testing (Vitest)
- **Hot Reload**: Sub-100ms updates for webview changes
- **Build Verification**: Multi-target validation (extension, MCP, webview)

### Performance Optimization

- **Compilation**: tsgo for 10x TypeScript speed, SWC for 20x build speed
- **Bundling**: Tree-shaking with ES2022, intelligent vendor chunking
- **Development**: Native ES modules, smart pre-bundling with esbuild
- **Production**: Advanced minification, CSS code splitting, asset optimization

### Cross-Platform Considerations

- **Path Handling**: path.posix for consistency across platforms
- **Node.js Compatibility**: Target Node.js 20 LTS for stability
- **Build Process**: Platform-specific optimizations, multi-platform testing
- **Native Dependencies**: Conditional exports, platform-specific builds

## Architectural Decisions

### VS Code Extension Integration

- **Activation Events**: Specific events, not wildcard
- **Command Namespacing**: Extension ID prefixes (aimemory.*)
- **Webview Security**: Strict CSP, nonce-based scripts, structured postMessage
- **Performance**: Lazy loading, proper disposal patterns, memory management

### MCP Server Architecture

- **Protocol Compliance**: @modelcontextprotocol/sdk 1.12+ standards
- **Transport**: stdio for Cursor compatibility (no HTTP/Express)
- **Service Layer**: Clean separation between MCP adapter and business logic
- **Error Handling**: Comprehensive boundaries with user-friendly messages

### Memory Bank Structure

- **Modular Organization**: core/, systemPatterns/, techContext/, progress/
- **Access Patterns**: Hot (always load), Warm (on-demand), Cold (chunked)
- **File Operations**: Async I/O, self-healing with templates, validation
- **Performance**: Intelligent caching, size limits, concurrent access safety

## Future Technology Considerations

### TypeScript Evolution

- **Migration Path**: tsgo → TypeScript 7.0 when feature-complete
- **Compatibility**: Maintain tsc fallback until full tsgo parity
- **Editor Support**: Monitor LSP improvements and feature additions
- **Performance**: Continue leveraging native compilation benefits

### Build System Evolution

- **Dependency Updates**: Pin critical tools, range for stable dependencies
- **Security**: Regular security patches via pnpm overrides
- **Performance Monitoring**: Bundle analysis, build speed metrics
- **Optimization**: Tree-shaking effectiveness, cache efficiency analysis

---

**Cross-References**: #typescript #tsgo #rollup #swc #vite #biome #vitest #performance #native-compilation

**See Also**: @002-build-system-tooling.mdc for actionable rules, docs/build-configuration.md for implementation details