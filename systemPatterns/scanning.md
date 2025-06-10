# Scanning

File discovery, validation, and health monitoring patterns in AI Memory Extension

## 🔍 File Discovery Patterns

### Memory Bank Structure Scanning

```typescript
// Systematic discovery of memory bank files
const MEMORY_BANK_STRUCTURE = {
  'core': [
    'projectBrief.md',
    'productContext.md',
    'activeContext.md'
  ],
  'systemPatterns': [
    'index.md',
    'architecture.md',
    'patterns.md',
    'scanning.md'
  ],
  'techContext': [
    'index.md',
    'stack.md',
    'dependencies.md',
    'environment.md'
  ],
  'progress': [
    'index.md',
    'current.md',
    'history.md'
  ]
} as const

// File type enumeration from structure
type MemoryBankFileType =
  | 'core/projectBrief.md'
  | 'core/productContext.md'
  | 'core/activeContext.md'
  // ... all combinations
```

### Directory Validation Pattern

```typescript
// Comprehensive directory structure validation
export async function validateMemoryBankDirectory(
  context: FileOperationContext
): Promise<boolean> {
  try {
    const stats = await context.fileOperationManager.stat(context.memoryBankPath)

    if (!stats.success) {
      context.logger.warn(`Memory bank directory does not exist: ${context.memoryBankPath}`)
      return false
    }

    if (!stats.data.isDirectory()) {
      context.logger.error(`Memory bank path is not a directory: ${context.memoryBankPath}`)
      return false
    }

    // Check required subdirectories
    const requiredDirs = ['core', 'systemPatterns', 'techContext', 'progress']
    for (const dir of requiredDirs) {
      const dirPath = path.join(context.memoryBankPath, dir)
      const dirStats = await context.fileOperationManager.stat(dirPath)

      if (!dirStats.success || !dirStats.data.isDirectory()) {
        context.logger.warn(`Required subdirectory missing or invalid: ${dir}`)
        return false
      }
    }

    return true
  } catch (error) {
    context.logger.error(`Error validating memory bank directory: ${error}`)
    return false
  }
}
```

## 📋 File Validation Patterns

### Individual File Validation

```typescript
// Validate single memory bank file
export async function validateMemoryBankFile(
  fileType: MemoryBankFileType,
  context: FileOperationContext
): Promise<{ isValid: boolean }> {
  const filePath = path.join(context.memoryBankPath, fileType)

  try {
    const stats = await context.fileOperationManager.stat(filePath)

    if (!stats.success) {
      context.logger.debug(`File does not exist: ${fileType}`)
      return { isValid: false }
    }

    // Check if file is readable
    const readResult = await context.fileOperationManager.readFile(filePath)
    if (!readResult.success) {
      context.logger.warn(`File exists but is not readable: ${fileType}`)
      return { isValid: false }
    }

    // Validate file content structure
    const contentValidation = validateFileContent(fileType, readResult.data)
    if (!contentValidation.isValid) {
      context.logger.warn(`File content validation failed: ${fileType}`)
      return { isValid: false }
    }

    return { isValid: true }
  } catch (error) {
    context.logger.error(`Error validating file ${fileType}: ${error}`)
    return { isValid: false }
  }
}
```

### Bulk File Validation

```typescript
// Validate all memory bank files efficiently
export async function validateAllMemoryBankFiles(
  context: FileOperationContext
): Promise<{ missingFiles: string[] }> {
  const missingFiles: string[] = []

  // Get all expected file types
  const allFileTypes = getAllMemoryBankFileTypes()

  // Validate files in parallel for performance
  const validationPromises = allFileTypes.map(async (fileType) => {
    const result = await validateMemoryBankFile(fileType, context)

    if (!result.isValid) {
      missingFiles.push(fileType)
    }

    return { fileType, ...result }
  })

  await Promise.all(validationPromises)

  context.logger.info(
    `Validation complete: ${missingFiles.length} missing`
  )

  return { missingFiles }
}
```

## 🏥 Health Monitoring Patterns

### Comprehensive Health Check

```typescript
// System health assessment
export async function performHealthCheck(
  context: FileOperationContext,
  files: Map<MemoryBankFileType, MemoryBankFile>
): Promise<string> {
  const healthReport: string[] = []

  // 1. Directory structure health
  const directoryHealth = await validateMemoryBankDirectory(context)
  healthReport.push(`Directory Structure: ${directoryHealth ? '✅ Valid' : '❌ Invalid'}`)

  // 2. File completeness health
  const { missingFiles } = await validateAllMemoryBankFiles(context)
  const completeness = missingFiles.length === 0
  healthReport.push(`File Completeness: ${completeness ? '✅ Complete' : `❌ Missing ${missingFiles.length} files`}`)

  // 4. File system permissions
  const permissionsHealth = await checkFileSystemPermissions(context)
  healthReport.push(`Permissions: ${permissionsHealth ? '✅ Valid' : '❌ Invalid'}`)

  return healthReport.join('\n')
}
```

### File System Permissions Check

```typescript
// Verify read/write permissions
async function checkFileSystemPermissions(
  context: FileOperationContext
): Promise<boolean> {
  try {
    // Test write permission with temporary file
    const testFile = path.join(context.memoryBankPath, '.health-check-temp')
    const testContent = `Health check: ${new Date().toISOString()}`

    const writeResult = await context.fileOperationManager.writeFile(testFile, testContent)
    if (!writeResult.success) {
      context.logger.warn('Write permission check failed')
      return false
    }

    // Test read permission
    const readResult = await context.fileOperationManager.readFile(testFile)
    if (!readResult.success || readResult.data !== testContent) {
      context.logger.warn('Read permission check failed')
      return false
    }

    // Clean up test file
    await fs.unlink(testFile).catch(() => {
      // Ignore cleanup errors
    })

    return true
  } catch (error) {
    context.logger.error(`Permission check error: ${error}`)
    return false
  }
}
```

## 📊 Metadata Scanning Patterns

### File Metadata Collection

```typescript
// Comprehensive file metadata gathering
interface FileMetadata {
  path: string
  size: number
  lastModified: Date
  isReadable: boolean
  contentHash?: string
  lineCount?: number
  wordCount?: number
}

export async function collectFileMetadata(
  fileType: MemoryBankFileType,
  context: FileOperationContext
): Promise<FileMetadata | null> {
  const filePath = path.join(context.memoryBankPath, fileType)

  try {
    const stats = await context.fileOperationManager.stat(filePath)
    if (!stats.success) {
      return null
    }

    const metadata: FileMetadata = {
      path: filePath,
      size: stats.data.size,
      lastModified: stats.data.mtime,
      isReadable: false
    }

    // Try to read file for additional metadata
    const readResult = await context.fileOperationManager.readFile(filePath)
    if (readResult.success) {
      metadata.isReadable = true
      metadata.contentHash = generateContentHash(readResult.data)
      metadata.lineCount = readResult.data.split('\n').length
      metadata.wordCount = readResult.data.split(/\s+/).length
    }

    return metadata
  } catch (error) {
    context.logger.error(`Error collecting metadata for ${fileType}: ${error}`)
    return null
  }
}
```

### Batch Metadata Scanning

```typescript
// Efficient metadata collection for all files
export async function scanAllFileMetadata(
  context: FileOperationContext
): Promise<Map<MemoryBankFileType, FileMetadata>> {
  const metadata = new Map<MemoryBankFileType, FileMetadata>()
  const allFileTypes = getAllMemoryBankFileTypes()

  // Collect metadata in parallel
  const metadataPromises = allFileTypes.map(async (fileType) => {
    const fileMetadata = await collectFileMetadata(fileType, context)
    if (fileMetadata) {
      metadata.set(fileType, fileMetadata)
    }
    return { fileType, metadata: fileMetadata }
  })

  await Promise.all(metadataPromises)

  context.logger.info(`Metadata scan complete: ${metadata.size} files processed`)
  return metadata
}
```

## 🔄 Change Detection Patterns

### File Change Monitoring

```typescript
// Detect changes since last scan
interface ChangeDetectionResult {
  modified: MemoryBankFileType[]
  added: MemoryBankFileType[]
  deleted: MemoryBankFileType[]
  unchanged: MemoryBankFileType[]
}

export async function detectChanges(
  context: FileOperationContext,
  lastScanMetadata: Map<MemoryBankFileType, FileMetadata>
): Promise<ChangeDetectionResult> {
  const currentMetadata = await scanAllFileMetadata(context)
  const result: ChangeDetectionResult = {
    modified: [],
    added: [],
    deleted: [],
    unchanged: []
  }

  // Check for modifications and additions
  for (const [fileType, current] of currentMetadata) {
    const previous = lastScanMetadata.get(fileType)

    if (!previous) {
      result.added.push(fileType)
    } else if (current.contentHash !== previous.contentHash) {
      result.modified.push(fileType)
    } else {
      result.unchanged.push(fileType)
    }
  }

  // Check for deletions
  for (const [fileType] of lastScanMetadata) {
    if (!currentMetadata.has(fileType)) {
      result.deleted.push(fileType)
    }
  }

  context.logger.info(
    `Change detection: ${result.modified.length} modified, ${result.added.length} added, ${result.deleted.length} deleted`
  )

  return result
}
```

### Smart Cache Invalidation

```typescript
// Intelligent cache invalidation based on changes
export function invalidateChangedFiles(
  changes: ChangeDetectionResult,
  context: FileOperationContext
): void {
  // Invalidate modified and deleted files
  const filesToInvalidate = [...changes.modified, ...changes.deleted]

  for (const fileType of filesToInvalidate) {
    context.cacheManager.invalidate(fileType)
    context.logger.debug(`Invalidated cache for: ${fileType}`)
  }

  // Update cache with new files
  for (const fileType of changes.added) {
    context.logger.debug(`New file detected: ${fileType}`)
    // Cache will be populated on next read
  }
}
```

---

> Last updated: 8 June 2025
