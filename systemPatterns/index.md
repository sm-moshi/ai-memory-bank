# System Patterns Index

> Summary of architectural patterns, design decisions, and system structure

## Architecture Overview
<!-- High-level system design -->
- **Pattern**: Clean Architecture with layered separation of concerns (utils → services → infrastructure → core)
- **Structure**: Modular TypeScript extension with React webview and stdio MCP server
- **Communication**: Event-driven async operations with explicit error handling via Result pattern

## Key Design Patterns
<!-- Primary patterns used throughout the system -->
- **Dependency Injection**: Services accept dependencies via constructor for testability and modularity
- **Result Pattern**: Explicit error handling with `Result<T, E>` throughout async operations
- **Streaming Pattern**: Intelligent file reading strategy (normal vs streaming based on size thresholds)
- **Template Pattern**: Consistent memory bank file generation with user-customizable templates
- **Observer Pattern**: Event-driven architecture for MCP server lifecycle and webview communication

## Service Architecture
<!-- How services are structured and interact -->
- **MemoryBankService**: Core business logic for memory bank operations and file management
- **FileOperationManager**: Async file operations with retry logic and streaming integration
- **StreamingManager**: Performance-optimised file reading with memory pressure management
- **SecurityValidator**: Input validation, path sanitisation, and resource limits enforcement
- **CacheManager**: LRU cache with bounded memory usage and automatic eviction

## Cross-Cutting Concerns
<!-- How the system handles common concerns -->
- **Error Handling**: Result pattern with explicit error types, secure error messages without sensitive data leakage
- **Logging**: Structured logging with configurable levels (trace, debug, info, warning, error)
- **Configuration**: Zero-configuration design with intelligent defaults and template-based auto-generation
- **Security**: Comprehensive Zod validation, path traversal prevention, resource limits, input sanitisation
- **Performance**: Streaming for large files (>1MB), bounded caching, memory pressure management, async operations

## Quality Patterns
<!-- Patterns ensuring code quality and maintainability -->
- **Testing Strategy**: Unit tests with Vitest, integration tests with @vscode/test-cli, >90% coverage maintenance
- **Build Pipeline**: Rollup + SWC for fast compilation, Biome for linting/formatting, parallel development workflow
- **Code Standards**: British English enforcement, strict TypeScript, consistent naming conventions (kebab-case files)
- **Documentation**: Template-based documentation generation, comprehensive formatting standards, professional presentation

## Development Workflow Patterns

### File Organisation Pattern
```
src/
├── core/              # Business logic & domain services
├── services/          # Domain-specific service modules
├── infrastructure/    # Framework adapters (VS Code, logging, process)
├── utils/             # Pure utilities (common, files, types)
├── types/             # TypeScript definitions
└── mcp/              # MCP server implementation
```

### Template System Pattern
- **Consistent Structure**: All templates follow established formatting rules
- **Placeholder System**: `[descriptive-name]` format for user customisation
- **Auto-generation**: Missing files created from templates with user context
- **Version Control**: Git-friendly structure supporting team collaboration

### Performance Optimization Pattern
- **Size-based Strategy**: Files <1MB use normal read, >=1MB use streaming
- **Memory Bounds**: Configurable cache limits with LRU eviction
- **Progress Feedback**: Optional streaming progress callbacks for UI
- **Resource Cleanup**: Explicit lifecycle management and cleanup

## Security Architecture Pattern

### Input Validation Layer
- **Zod Schemas**: Runtime type validation for all inputs
- **Path Security**: Traversal prevention, file system boundaries
- **Resource Limits**: File size, cache size, operation timeouts
- **Error Security**: No sensitive data in error messages

### MCP Server Security
- **Stdio Transport**: Exclusive use of stdio for Cursor compatibility
- **Process Isolation**: Separate process for MCP server operations
- **Input Sanitisation**: All MCP tool parameters validated and sanitised

## Documentation Standards Pattern

### Template Formatting Rules
- **Descriptions**: Use blockquotes (`>`) not emphasis (`*`) for document descriptions
- **Timestamps**: Blockquote format for "Last updated" metadata
- **Status Indicators**: Emoji + bold labels (✅ **COMPLETE**, 🔄 **IN PROGRESS**)
- **Code Blocks**: Proper language specification (`markdown`, `typescript`, `bash`)

### Memory Bank Structure
```
memory-bank/
├── core/              # Project brief, product context, active context
├── progress/          # Current progress, history tracking
├── systemPatterns/    # Architecture patterns, design decisions
└── techContext/       # Technology stack, dependencies, environment
```

## Integration Patterns

### VS Code Extension Pattern
- **Activation Events**: Specific command-based activation (not `*`)
- **Command Namespacing**: `aimemory.*` prefix for all commands
- **Webview Security**: CSP compliance, nonce-based inline content
- **Configuration**: Contribute settings with validation and defaults

### Build System Pattern
- **Multi-Target**: Extension (CommonJS), MCP CLI (CommonJS), Webview (ES modules)
- **Development**: Parallel builds with hot reload (extension + webview)
- **Production**: Optimised builds with minification and source maps
- **Quality Gates**: Type checking + linting + testing before packaging

## Future Evolution Patterns

### Phase 2 Metadata System
- **Content Analysis**: Intelligent file categorisation and tagging
- **Search Architecture**: Fast indexing and querying across memory bank content
- **User Interface**: Enhanced webview with search, filtering, and organisation tools

### Scalability Considerations
- **Large Memory Banks**: Handling >100MB with continued performance
- **Multi-workspace**: Support for multiple project memory banks
- **Cloud Integration**: Future remote/cloud memory bank synchronisation

---
> *Last updated: 2025-05-31*