# Model Context Protocol (MCP)

## What It Is

An open standard (by Anthropic) for connecting AI assistants to external tools and data sources. Think of it as **USB for AI tools** — a standard interface so any assistant can use any tool.

## Why It Matters

Before MCP, every AI tool had its own integration method. Want Claude Code to access your database? Write a custom tool. Want it to search Slack? Another custom integration. MCP standardizes this:

```
Without MCP:                    With MCP:
┌────────┐  custom  ┌────┐     ┌────────┐  MCP   ┌────────────┐
│ Claude │─────────→│ DB │     │ Claude │───────→│ MCP Server │
│  Code  │  custom  ├────┤     │  Code  │        │ (any tool) │
│        │─────────→│Slack│    │        │        └────────────┘
│        │  custom  ├────┤     │ Cursor │  MCP   ┌────────────┐
│        │─────────→│FS  │     │        │───────→│ Same server│
└────────┘          └────┘     └────────┘        └────────────┘
```

## Protocol Details

### Transport

Two transport modes:
- **stdio**: Server runs as a local process. Communication via stdin/stdout. Best for local tools.
- **SSE (Server-Sent Events)**: Server runs as HTTP service. Best for remote/shared tools.

### Message Format

JSON-RPC 2.0 over the transport layer:

```json
// Client → Server: tool call
{"jsonrpc": "2.0", "method": "tools/call", "params": {"name": "query_db", "arguments": {"sql": "SELECT..."}}, "id": 1}

// Server → Client: result
{"jsonrpc": "2.0", "result": {"content": [{"type": "text", "text": "..."}]}, "id": 1}
```

### Capabilities

An MCP server exposes:
- **Tools**: Functions the model can call (like API endpoints)
- **Resources**: Data the model can read (like files or database records)
- **Prompts**: Reusable prompt templates

## Building an MCP Server

Minimal Python server using the official SDK:

```python
from mcp.server import Server
from mcp.types import Tool, TextContent

server = Server("my-server")

@server.list_tools()
async def list_tools():
    return [Tool(name="greet", description="Say hello", inputSchema={...})]

@server.call_tool()
async def call_tool(name, arguments):
    if name == "greet":
        return [TextContent(type="text", text=f"Hello, {arguments['name']}!")]
```

## Configuration

In Claude Code (`~/.claude/settings.json`):
```json
{
  "mcpServers": {
    "my-server": {
      "command": "python",
      "args": ["path/to/server.py"],
      "type": "stdio"
    }
  }
}
```

## Ecosystem

- [Official servers repo](https://github.com/modelcontextprotocol/servers) — reference implementations
- [MCP specification](https://spec.modelcontextprotocol.io/) — full protocol spec
- Supported by: Claude Code, Cursor, Windsurf, Continue, and growing
