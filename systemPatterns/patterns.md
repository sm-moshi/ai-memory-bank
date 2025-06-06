# Patterns

> Real implementation patterns and practices used in AI Memory Extension

## 🎯 Core Design Patterns

### **Result Pattern** (Error Handling)

```typescript
// Explicit error handling throughout the codebase
type AsyncResult<T, E = Error> = Promise<
  | { success: true; data: T }
  | { success: false; error: E }
>

// Usage example from MemoryBankServiceCore
async getIsMemoryBankInitialized(): AsyncResult<boolean, MemoryBankError> {
  const result = await tryCatchAsync<boolean>(async () => {
    const context = this.createContext()
    const isValidDirectory = await validateMemoryBankDirectory(context)
    return isValidDirectory && missingFiles.length === 0
  })

  if (isError(result)) {
    return {
      success: false,
      error: new MemoryBankError(
        `Operation failed: ${result.error.message}`,
        originalErrorCode,
        { originalError: result.error }
      )
    }
  }

  return result
}
```

**Benefits**:

- **Explicit Error Handling**: No hidden exceptions
- **Type Safety**: Compiler enforces error checking
- **Composability**: Results can be chained and transformed
- **Debugging**: Clear error propagation paths

### **Dependency Injection Pattern**

```typescript
// DIContainer for service management
class DIContainer {
  private services = new Map<string, any>()
  private singletons = new Map<string, any>()

  register<T>(name: string, factory: (container: DIContainer) => T, singleton = false): void
  resolve<T>(name: string): T
}

// Service registration in extension.ts
function registerCoreServices(container: DIContainer, logger: Logger, context: vscode.ExtensionContext) {
  container.register("Logger", () => logger, true)
  container.register("CacheManager", c => new CacheManager(c.resolve("Logger")), true)
  container.register("FileOperationManager", () => fileOperationManager, true)
  container.register("MemoryBankServiceCore", c =>
    new MemoryBankServiceCore(
      memoryBankPath,
      c.resolve("Logger"),
      c.resolve("CacheManager"),
      c.resolve("StreamingManager"),
      c.resolve("FileOperationManager")
    ), true)
}
```

**Benefits**:

- **Testability**: Easy to mock dependencies
- **Flexibility**: Runtime service configuration
- **Lifecycle Management**: Singleton pattern for shared services
- **Decoupling**: Services don't know about concrete implementations

### **Adapter Pattern** (Legacy Integration)

```typescript
// Bridge between new and legacy systems
class LegacyCacheAdapter {
  constructor(private modernCache: CacheManager) {}

  // Legacy interface implementation
  get(key: string): any {
    return this.modernCache.get(key)
  }

  set(key: string, value: any): void {
    this.modernCache.set(key, value)
  }

  // Additional legacy methods...
}

// Usage in MemoryBankServiceCore
constructor(...) {
  this._legacyCacheAdapter = new LegacyCacheAdapter(cacheManager)
  this._legacyStatsAdapter = new LegacyStatsAdapter(cacheManager)
}
```

**Benefits**:

- **Gradual Migration**: Smooth transition between systems
- **Interface Compatibility**: Maintain existing APIs
- **Performance**: No performance penalty for adaptation
- **Risk Reduction**: Incremental changes vs big bang rewrites

## 🔧 Service Layer Patterns

### **Service Composition Pattern**

```typescript
// Core service composes multiple specialized services
class MemoryBankServiceCore {
  constructor(
    private memoryBankPath: string,
    private logger: MemoryBankLogger,
    private cacheManager: CacheManager,
    private streamingManager: StreamingManager,
    private fileOperationManager: FileOperationManager
  ) {}

  // High-level operations delegate to specialized services
  async loadFiles(): AsyncResult<MemoryBankFileType[], MemoryBankError> {
    const context = this.createContext()
    const createdFiles = await loadAllMemoryBankFiles(context, this.files)
    this.ready = true
    return { success: true, data: createdFiles }
  }
}
```

### **Context Pattern** (Operation Context)

```typescript
// Shared context for file operations
interface FileOperationContext {
  memoryBankPath: string
  logger: MemoryBankLogger
  cacheManager: CacheManager
  streamingManager: StreamingManager
  fileOperationManager: FileOperationManager
}

// Context creation in service
private createContext(): FileOperationContext {
  return {
    memoryBankPath: this._memoryBankFolder,
    logger: this.logger,
    cacheManager: this.cacheManager,
    streamingManager: this.streamingManager,
    fileOperationManager: this.fileOperationManager
  }
}
```

**Benefits**:

- **Parameter Reduction**: Avoid long parameter lists
- **Consistency**: Same context across related operations
- **Extensibility**: Easy to add new context properties
- **Testing**: Simplified mock setup

## 🔄 Async Patterns

### **Retry Pattern with Exponential Backoff**

```typescript
// FileOperationManager retry logic
async mkdirWithRetry(
  dirPath: string,
  options?: { recursive?: boolean },
  maxRetries = 3
): Promise<FileResult<void>> {
  let lastError: Error | null = null

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      await fs.mkdir(dirPath, options)
      return { success: true, data: undefined }
    } catch (error) {
      lastError = error as Error

      if (attempt < maxRetries) {
        const delay = Math.pow(2, attempt - 1) * 100 // Exponential backoff
        await new Promise(resolve => setTimeout(resolve, delay))
        this.logger.warn(`Retry attempt ${attempt} for mkdir ${dirPath}`)
      }
    }
  }

  return {
    success: false,
    error: new FileOperationError(`Failed after ${maxRetries} attempts`, lastError)
  }
}
```

### **Streaming Pattern** (Large File Handling)

```typescript
// StreamingManager for intelligent file reading
class StreamingManager {
  async readFile(filePath: string): Promise<FileContent> {
    const stats = await fs.stat(filePath)

    if (this.shouldUseStreaming(stats.size)) {
      return this.readFileStreaming(filePath)
    } else {
      return this.readFileNormal(filePath)
    }
  }

  private shouldUseStreaming(fileSize: number): boolean {
    return fileSize > this.STREAMING_THRESHOLD // 30KB
  }

  private async readFileStreaming(filePath: string): Promise<FileContent> {
    const chunks: Buffer[] = []
    const stream = fs.createReadStream(filePath)

    return new Promise((resolve, reject) => {
      stream.on('data', chunk => chunks.push(chunk))
      stream.on('end', () => resolve({
        content: Buffer.concat(chunks).toString('utf-8'),
        isStreamed: true
      }))
      stream.on('error', reject)
    })
  }
}
```

## 🛡 Security Patterns

### **Input Validation Pattern** (Zod Schemas)

```typescript
// Runtime validation with Zod
import { z } from 'zod'

const MemoryBankFileTypeSchema = z.enum([
  'core/projectBrief.md',
  'core/productContext.md',
  'core/activeContext.md',
  // ... all valid file types
])

const UpdateFileRequestSchema = z.object({
  fileType: MemoryBankFileTypeSchema,
  content: z.string().min(1, "Content cannot be empty"),
  metadata: z.object({
    lastModified: z.string().datetime().optional(),
    author: z.string().optional()
  }).optional()
})

// Usage in MCP tools
export async function updateMemoryBankFile(args: unknown) {
  const validatedArgs = UpdateFileRequestSchema.parse(args)
  // Now validatedArgs is type-safe and validated
  return await memoryBank.updateFile(validatedArgs.fileType, validatedArgs.content)
}
```

### **Path Security Pattern**

```typescript
// Path traversal prevention
function validateAndConstructArbitraryFilePath(
  basePath: string,
  relativePath: string
): Result<string, SecurityError> {
  // Normalize and resolve path
  const normalizedPath = path.normalize(relativePath)
  const fullPath = path.resolve(basePath, normalizedPath)

  // Ensure path is within base directory
  if (!fullPath.startsWith(path.resolve(basePath))) {
    return {
      success: false,
      error: new SecurityError("Path traversal attempt detected", "PATH_TRAVERSAL")
    }
  }

  // Additional security checks
  if (containsInvalidCharacters(normalizedPath)) {
    return {
      success: false,
      error: new SecurityError("Invalid characters in path", "INVALID_PATH")
    }
  }

  return { success: true, data: fullPath }
}
```

## 🎨 UI Patterns

### **Message Passing Pattern** (Extension ↔ Webview)

```typescript
// Structured communication between extension and webview
interface WebviewMessage {
  type: 'status' | 'command' | 'error' | 'update'
  payload: any
  id?: string // For request/response correlation
}

// Extension side
private setupMessageHandling(webview: vscode.Webview): void {
  webview.onDidReceiveMessage(async (message: WebviewMessage) => {
    switch (message.type) {
      case 'command':
        const result = await this.handleCommand(message.payload)
        webview.postMessage({
          type: 'response',
          payload: result,
          id: message.id
        })
        break
      // ... other message types
    }
  })
}

// Webview side (React)
const sendCommand = useCallback((command: string, args: any[]) => {
  const messageId = generateId()
  vscode.postMessage({
    type: 'command',
    payload: { command, args },
    id: messageId
  })

  // Wait for response...
}, [])
```

### **React Hook Patterns** (Webview)

```typescript
// Custom hooks for webview state management
function useMemoryBankStatus() {
  const [status, setStatus] = useState<'loading' | 'ready' | 'error'>('loading')
  const [serverRunning, setServerRunning] = useState(false)

  useEffect(() => {
    // Listen for status updates from extension
    const handleMessage = (event: MessageEvent) => {
      if (event.data.type === 'status') {
        setStatus(event.data.payload.status)
        setServerRunning(event.data.payload.serverRunning)
      }
    }

    window.addEventListener('message', handleMessage)
    return () => window.removeEventListener('message', handleMessage)
  }, [])

  return { status, serverRunning }
}
```

## 🧪 Testing Patterns

### **Mock Factory Pattern**

```typescript
// Centralized mock creation for consistent testing
export class MockFactory {
  static createMemoryBankService(): jest.Mocked<MemoryBankServiceCore> {
    return {
      getIsMemoryBankInitialized: jest.fn(),
      loadFiles: jest.fn(),
      updateFile: jest.fn(),
      checkHealth: jest.fn(),
      // ... all methods mocked
    } as jest.Mocked<MemoryBankServiceCore>
  }

  static createFileOperationManager(): jest.Mocked<FileOperationManager> {
    return {
      readFile: jest.fn(),
      writeFile: jest.fn(),
      mkdirWithRetry: jest.fn(),
      // ... all methods mocked
    } as jest.Mocked<FileOperationManager>
  }
}

// Usage in tests
describe('MemoryBankServiceCore', () => {
  let service: MemoryBankServiceCore
  let mockFileManager: jest.Mocked<FileOperationManager>

  beforeEach(() => {
    mockFileManager = MockFactory.createFileOperationManager()
    service = new MemoryBankServiceCore(
      '/test/path',
      mockLogger,
      mockCache,
      mockStreaming,
      mockFileManager
    )
  })
})
```

### **MSW API Mocking Pattern**

```typescript
// Modern MSW 2.9.0 patterns for API testing
import { http, HttpResponse } from 'msw'
import { setupServer } from 'msw/node'

const handlers = [
  http.get('/api/memory-bank/status', () => {
    return HttpResponse.json({
      status: 'healthy',
      filesCount: 14,
      lastUpdate: new Date().toISOString()
    })
  }),

  http.post('/api/memory-bank/update', async ({ request }) => {
    const body = await request.json()
    return HttpResponse.json({
      success: true,
      fileType: body.fileType,
      updated: new Date().toISOString()
    })
  })
]

const server = setupServer(...handlers)

// Test setup
beforeAll(() => server.listen())
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```

---

> Last updated: 6 June 2025
