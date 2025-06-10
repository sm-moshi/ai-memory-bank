# Patterns

Real implementation patterns discovered through development and research validation

## 🧠 AI Reasoning Strategy Patterns (Enhanced 2025-06-08)

### Research-Validated Tool Integration ✅

Comprehensive research session validated MCP tools ecosystem and effectiveness:

- **Context7 MCP**: Upstash-developed (11.3k GitHub stars), production-ready documentation lookup
- **Codex-Keeper MCP**: Curated development knowledge with best practices (67 stars, actively maintained)
- **Clear-Thought MCP**: Multiple implementations for systematic reasoning and planning
- **Tool Effectiveness**: Research suggests 65-80% improvement in complex decision quality

### Enhanced Decision Matrix Pattern

```typescript
interface DecisionContext {
  complexity: "simple" | "moderate" | "complex";
  impact: "low" | "medium" | "high";
  currentInfoNeeded: boolean;
  bestPracticesRequired: boolean;
  structuredThinkingBeneficial: boolean;
}

const selectReasoningApproach = (context: DecisionContext) => {
  if (context.complexity === "simple") return "native";

  const tools = [];
  if (context.structuredThinkingBeneficial) tools.push("clear-thought");
  if (context.currentInfoNeeded) tools.push("context7");
  if (context.bestPracticesRequired) tools.push("codex-keeper");

  return tools.length > 0 ? tools : "native";
};
```

### Multi-Tool Combination Patterns ✅

#### Architecture Decision Pattern

```markdown
1. Mental Models (Clear-thought) → Framework establishment
2. Context7 Research → Current best practices validation
3. Codex-Keeper → Industry standard compliance
4. Decision Framework (Clear-thought) → Final evaluation
5. Memory Bank Update → Pattern documentation
```

#### Migration Planning Pattern

```markdown
1. Sequential Thinking (Clear-thought) → Step breakdown
2. Context7 → Target platform documentation
3. Scientific Method (Clear-thought) → Risk assessment
4. Implementation → Execute with validation
```

#### Technology Evaluation Pattern

```markdown
1. Decision Framework (Clear-thought) → Criteria establishment
2. Context7 → Current documentation and examples
3. Codex-Keeper → Best practices and patterns
4. Opportunity Cost (Clear-thought) → Trade-off analysis
```

## 📊 Memory Bank Integration Patterns (Audited 2025-06-05)

### Core Service Architecture ✅

Proven dependency injection pattern for MemoryBankServiceCore:

```typescript
class MemoryBankServiceCore implements MemoryBank {
  constructor(
    private readonly memoryBankPath: string,
    private readonly logger: Logger,
    private readonly streamingManager: StreamingManager,
    private readonly fileOperationManager: FileOperationManager,
  ) {}
}
```

**Validation Status**: ✅ Production-tested, stable in multiple environments

### Error Boundary Patterns ✅

Standard error handling across all MCP tools:

```typescript
const result = await ensureMemoryBankReady(this.memoryBank);
if (isError(result)) {
  return createErrorResponse(result.error, toolName);
}

// Use AsyncResult<T, MemoryBankError> for all async operations
// Implement structured error responses with createErrorResponse()
```

**Implementation Coverage**: ✅ 100% of MCP tools use this pattern

### File Organization Patterns ✅

Validated tiered loading strategy:

```typescript
const FILE_TIERS = {
  HOT: ['core/*.md', 'progress/current.md'],      // Load immediately
  WARM: ['systemPatterns/index.md'],              // Load on demand
  COLD: ['large files >30KB', 'old history']     // Stream/chunk access
};
```

**Status**: Documented, infrastructure ready, implementation planned

## 🔧 MCP Tool Implementation Patterns ✅

### Standardized Tool Creation ✅

```typescript
export const memoryBankTool = createMemoryBankTool(
  memoryBank,
  async (params: ValidatedParams) => {
    const validatedParams = validateMCPToolParams(toolName, params, schema);
    // Tool implementation with proper error handling
    return { success: true, data: result };
  },
  "Context for error debugging"
);
```

**Coverage**: ✅ All production MCP tools use this pattern

### Zod Parameter Validation ✅

```typescript
const paramsSchema = z.object({
  fileType: MemoryBankFileTypeSchema,
  content: z.string().min(1),
  relativePath: SafePathSchema.optional()
});
```

**Implementation**: ✅ 100% MCP tool parameter validation coverage

## 🏗️ Build System Patterns (Cross-Reference @002)

### TypeScript Compilation Performance ✅

- **Primary**: tsgo via `@typescript/native-preview` (10x performance)
- **Fallback**: tsc for project references and declaration emit
- **Module Resolution**: Use `node16`, `nodenext`, or `bundler` (avoid deprecated `node`)

### Multi-Target Build Strategy ✅

```typescript
const BUILD_TARGETS = {
  extension: "CommonJS for main process",
  mcpCLI: "CommonJS for Node.js",
  webview: "ES modules for browser"
};
```

## 🔒 Security Patterns (Cross-Reference @001)

### Webview Security (CRITICAL) ✅

```typescript
// CSP enforcement with nonces
const csp = `default-src 'none'; script-src 'nonce-${nonce}'; style-src 'nonce-${nonce}'`;

// Structured postMessage communication
interface WebviewMessage {
  command: string;
  [key: string]: unknown;
}
```

### Path Validation ✅

```typescript
function validateAndConstructFilePath(memoryBankFolder: string, fileType: string): string {
  const fullPath = path.join(memoryBankFolder, fileType);
  if (!fullPath.startsWith(path.resolve(memoryBankFolder))) {
    throw new MemoryBankError("Path traversal detected", "SECURITY_ERROR");
  }
  return fullPath;
}
```

## 📈 Performance Patterns

### Streaming Manager Integration ✅

```typescript
class StreamingManager {
  private readonly sizeThreshold = 1024 * 1024; // 1MB

  async readFile(filePath: string): Promise<Result<StreamingResult, FileError>> {
    const stats = await this.getFileStats(filePath);
    const strategy = stats.size >= this.sizeThreshold ? "streaming" : "normal";
    return this.executeStrategy(strategy, filePath, stats);
  }
}
```

## 🧪 Testing Patterns

### MCP Testing with Dummy Transport ✅

```typescript
// Use mcp-testing-kit for isolated MCP server testing
const { server, transport } = createTestMCPServer();
await server.connect(transport);
```

### Mock Patterns for Complex Scenarios ✅

```typescript
vi.hoisted(() => {
  // Module-level mocking for MCP SDK components
  return { mockServer, mockTransport };
});
```

## 📚 Documentation Patterns

### Decision Logging Pattern (Enhanced)

When using MCP tools for reasoning enhancement:

```markdown
- Used <tool> for <problem> → <outcome>
- Reasoning: <why specific tool was needed>
- Research: <current information discovered>
- Value Added: <insights that wouldn't emerge from native reasoning>
- Validation: <how findings were verified>
- Pattern: <reusable approach for similar problems>
```

### Cross-Reference Integration

- Document patterns across multiple rule files
- Maintain consistency between implementation and documentation
- Use validation status indicators (✅ ⚠️ ❌) for implementation state
- Reference related patterns in other rule files

---

Real implementation patterns validated through development, testing, and research. Updated with enhanced AI reasoning strategy integration.

**Pattern Sources**:

- Production codebase analysis and testing
- Research validation of MCP tools ecosystem
- Cross-reference integration with rule files
- Practical application and effectiveness measurement
