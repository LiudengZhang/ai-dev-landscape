# Claude Code Deep Dive

## What It Is

Anthropic's agentic coding CLI. Not a code completion tool — a full agent that reads files, writes code, runs commands, uses external tools via MCP, and follows reusable patterns via skills.

## Architecture

```
┌─────────────────────────────────────────┐
│              Claude Code CLI            │
├───────────┬───────────┬─────────────────┤
│   Tools   │   Skills  │     Hooks       │
│ (built-in)│ (prompts) │ (shell events)  │
├───────────┴───────────┴─────────────────┤
│           MCP Servers                    │
│  (stdio/SSE connections to external     │
│   tools, APIs, databases)               │
├─────────────────────────────────────────┤
│           Claude API                     │
│  (model inference, tool calling)         │
└─────────────────────────────────────────┘
```

## Core Concepts

### Tools (Built-in)

Claude Code has built-in tools for file operations, shell commands, search:
- `Read` — read files
- `Write` — create files
- `Edit` — modify files (string replacement)
- `Bash` — run shell commands
- `Glob` — find files by pattern
- `Grep` — search file contents
- `Agent` — spawn subagents for parallel/isolated tasks

### Skills

Markdown files that extend Claude Code's capabilities. Stored in `~/.claude/skills/` or project `.claude/skills/`.

A skill is a structured prompt template that Claude Code loads when invoked. Skills can:
- Define workflows (e.g., "build an awesome list")
- Codify patterns (e.g., "how to review a PR in this project")
- Automate multi-step processes

### Hooks

Shell commands triggered by Claude Code events:
- `PreToolUse` — before a tool runs
- `PostToolUse` — after a tool runs
- `Notification` — on notifications

Use cases: auto-format on file write, lint on save, notify on completion.

### MCP Servers

External tools connected via the Model Context Protocol:
- **stdio**: Local process, communicates via stdin/stdout
- **SSE**: Remote server, communicates via HTTP Server-Sent Events

Examples: database access, API clients, documentation fetchers, web search.

### Subagents

Spawn isolated agent instances for:
- Parallel research (multiple searches at once)
- Isolated tasks (don't pollute main context)
- Specialized work (different model per agent)

Key: subagents get fresh context. You construct what they need, they don't inherit your history.

## Configuration

```
~/.claude/
├── settings.json       # Global settings (MCP servers, permissions)
├── skills/             # Global skills
└── projects/
    └── <project-hash>/
        ├── CLAUDE.md   # Project-specific instructions
        └── memory/     # Persistent memory across conversations

.claude/                # Per-project
├── settings.json       # Project MCP servers
├── skills/             # Project skills
└── CLAUDE.md           # Project instructions (checked into git)
```

## Patterns I've Learned

1. **Skills > repeated instructions**: If you explain a workflow more than twice, make it a skill.
2. **Subagents for research, main context for decisions**: Don't pollute your context with search results.
3. **MCP for external tools**: Don't shell out to curl when you can have a proper MCP server.
4. **Memory for cross-conversation context**: User preferences, project state, feedback loops.
5. **CLAUDE.md for team conventions**: Checked into git, everyone gets the same instructions.
