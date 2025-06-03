# Design Patterns & Best Practices

## MCP Server Implementation Patterns

### Tool Registration Pattern

**Context**: MCP tools require specific registration patterns with proper type safety and response structures.

**Implementation Pattern**:

```typescript
// Correct server.tool registration pattern
this.server.tool(
  "tool-name",
  "Clear description of what the tool does",
  paramsSchema.shape, // Use .shape for Zod schemas
  async (params) => {
    // Validate inputs
    const result = paramsSchema.safeParse(params);
    if (!result.success) {
      return createErrorResponse(`Invalid parameters: ${formatZodError(result.error)}`);
    }

    // Business logic
    const data = await performOperation(result.data);

    // Return structured response
    return createSuccessResponse({
      content: formatContentForMCP(data),
      metadata: extractMetadata(data)
    });
  }
);
```

**Anti-patterns**:

- ❌ Using type-only imports for runtime values (`import type { EnumType }`)
- ❌ Incorrect server.tool signature (wrong number of parameters)
- ❌ Returning Promise types instead of structured content

### Error Response Pattern

**Standard MCP Error Response**:

```typescript
interface MCPErrorResponse {
  content: string;
  isError: true;
  errorCode?: string;
  details?: any;
}

function createErrorResponse(message: string, code?: string): MCPErrorResponse {
  return {
    content: message,
    isError: true,
    errorCode: code,
  };
}
```

### Success Response Pattern

**Standard MCP Success Response**:

```typescript
interface MCPSuccessResponse {
  content: string;
  metadata?: FrontmatterMetadata;
  validationStatus?: string;
  validationErrors?: ZodIssue[];
  actualSchemaUsed?: string;
}

function createSuccessResponse(data: MCPSuccessResponse): MCPSuccessResponse {
  return {
    content: data.content,
    ...data
  };
}
```

## Validation Error Formatting

### Human-Readable Zod Errors

**Pattern**: Convert Zod validation errors to user-friendly messages

```typescript
function formatZodError(error: ZodError): string {
  const issues = error.issues.map(issue => {
    const path = issue.path.length > 0 ? ` at '${issue.path.join('.')}'` : '';
    return `${issue.message}${path}`;
  });

  return issues.length === 1
    ? issues[0]
    : `Multiple validation errors:\n${issues.map(issue => `  - ${issue}`).join('\n')}`;
}
```

**Use Cases**:

- Frontmatter validation error reporting
- MCP tool parameter validation
- File schema validation feedback

## Import Organization Patterns

### Type vs Runtime Imports

**Correct Pattern**:

```typescript
// Runtime imports for values used at runtime
import { MemoryBankFileType } from "../types/core.js";

// Type-only imports for types used only in type annotations
import type { CoreMemoryBankConfig } from "../types/mcpTypes.js";
```

**Investigation Notes**:

- TypeScript error `'MemoryBankFileType' cannot be used as a value because it was imported using 'import type'` occurs when enums or values are imported as type-only
- Solution: Use runtime imports for values, type-only imports for interfaces/types

### Module Resolution Strategy

**Research Finding (2025-05-31)**:

- MCP server types in `@modelcontextprotocol/sdk` require specific response formats
- `server.tool` method signature expects 2-5 arguments, not 6
- Response format must be structured text content, not raw strings

## Architecture Decision Records

### ADR-001: MCP Tool Response Format

**Decision**: Use helper functions for consistent response formatting

**Context**: MCP tools need consistent response structures for proper integration

**Consequences**:

- ✅ Consistent error handling across all tools
- ✅ Proper metadata integration
- ✅ Type safety for responses
- ❌ Additional abstraction layer complexity

### ADR-002: Error Handling Strategy

**Decision**: Use `Result<T, E>` pattern for internal operations, convert to MCP responses at boundaries

**Context**: Need consistent error handling that works with both internal operations and MCP protocol

**Implementation**:

```typescript
// Internal operation
async function readMemoryFile(path: string): Promise<Result<MemoryBankFile, FileError>> {
  // Implementation with Result pattern
}

// MCP tool boundary
async function mcpReadFile(params: any): Promise<MCPResponse> {
  const result = await readMemoryFile(params.filePath);

  if (!result.success) {
    return createErrorResponse(result.error.message);
  }

  return createSuccessResponse({
    content: result.data.content,
    metadata: result.data.metadata
  });
}
```

## Testing Patterns

### MCP Tool Testing Strategy

**Mock MCP Server**:

```typescript
const mockServer = {
  tool: vi.fn(),
  close: vi.fn(),
};
```

**Response Validation**:

```typescript
// Test success responses
expect(response.isError).toBeUndefined();
expect(response.content).toBeDefined();

// Test error responses
expect(response.isError).toBe(true);
expect(response.content).toContain("error message");
```

## Performance Patterns

### File Operation Strategy

**Pattern**: Always use FileOperationManager for memory bank files, direct fs for system config files

```typescript
// Memory bank files - use performance layer
const result = await this.fileOperationManager.readFileWithRetry(memoryBankPath);

// System config files - use direct fs (optimal for ~/.cursor/mcp.json)
const config = await fs.readFile(configPath, 'utf-8');
```

**Rationale**: VS Code extensions have no sandbox restrictions for home directory access, performance layer is optimized for memory bank operations specifically.

---

> *Last updated: 2025-05-31 with MCP investigation findings*
