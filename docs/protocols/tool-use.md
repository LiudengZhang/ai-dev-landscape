# Tool Use Patterns

## The Core Pattern

Every AI tool-use system follows the same loop:

```
1. Model receives: system prompt + messages + tool definitions
2. Model outputs: either text OR a tool call (structured JSON)
3. Host executes the tool call
4. Host feeds result back as a new message
5. Goto 1
```

This is the fundamental pattern behind Claude Code, Codex, all agent frameworks.

## Tool Definition

Tools are defined as JSON schemas that tell the model what's available:

```json
{
  "name": "read_file",
  "description": "Read contents of a file",
  "input_schema": {
    "type": "object",
    "properties": {
      "path": {"type": "string", "description": "File path to read"}
    },
    "required": ["path"]
  }
}
```

The model never executes tools — it outputs a structured call, and the host decides whether to execute.

## Provider Differences

| Provider | Tool Call Format | Parallel Calls | Streaming |
|----------|-----------------|---------------|-----------|
| Anthropic | `tool_use` content block | Yes | Yes |
| OpenAI | `tool_calls` in message | Yes | Yes |
| Google | `functionCall` part | Yes | Yes |

The formats differ slightly but the pattern is identical.

## Design Principles

1. **Tools should be atomic**: One tool, one action. Don't combine "read and modify" into one tool.
2. **Descriptions matter more than names**: The model reads the description to decide when to use a tool.
3. **Schema is the contract**: Be specific about types, required fields, and valid values.
4. **Fewer tools > many tools**: Models perform better with fewer, well-defined tools than many overlapping ones.
5. **Let the model decide**: Don't force tool use — let the model choose based on the task.

## Structured Outputs

A related pattern: constrain the model to output valid JSON matching a schema. Not a tool call — just structured response format.

```json
{
  "response_format": {
    "type": "json_schema",
    "json_schema": {
      "name": "analysis",
      "schema": {
        "type": "object",
        "properties": {
          "score": {"type": "integer"},
          "reasoning": {"type": "string"}
        }
      }
    }
  }
}
```

Used in: the Ollama issue screening script (structured JSON output from local model).
