# Scanning Patterns

Describes how the system reads/writes files and streams large content while maintaining speed & security.

## File IO Strategy

- **Centralised IO**: All disk operations funnel through `FileOperationManager`.
- **Retries & Back-off**: `FileOperationManager` wraps every FS call (`fs/promises`) with up to 3 retries and exponential back-off (`100 ms → 200 ms → 400 ms`, capped at 5 s).
- **Timeouts**: Each operation defaults to a 30 s timeout to avoid hanging IO.
- **Path Validation**: Every public method first calls `validateMemoryBankPath()` to prevent directory traversal attacks.

## Large File Handling

### Priority-Based Loading (Hot / Warm / Cold)

The **tiered strategy remains conceptually** useful even though we don't rely on heavy streaming or caching layers:

| Tier | Typical Size | Load Strategy |
|------|--------------|---------------|
| **Hot**  | ≤5 KB | Load eagerly at MCP server start-up and keep in memory. |
| **Warm** | 5-15 KB | Lazy-load on first access, then cache in memory for the session. |
| **Cold** | ≥15 KB | Read from disk on every access; no streaming – plain `fs.readFile` is fast enough for these docs. |

This keeps the implementation simple (just conditional reads) while still preventing unnecessary I/O for rarely used, larger documents.

## Memory-Bank Scanning

- **Directory Walk**: When Cursor requests the memory-bank root, `MemoryBankManager.getAllFiles()` enumerates the directory using safe `fs.readdir` calls.
- **Metadata**: Each file's `lastUpdated` timestamp is returned so Cursor (or the webview) can decide whether to refresh caches.

## Performance Considerations

- Blocking IO is avoided inside the webview process; heavy IO occurs in the extension host or MCP server process.
- Re-use of `FileOperationManager` avoids duplicated validation logic and centralises logging for IO metrics.

---
> *Last updated: 2025-06-11*
