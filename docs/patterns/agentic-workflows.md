# Agentic Workflows

## What Makes Something "Agentic"?

An AI workflow is agentic when the model **makes decisions about what to do next** rather than following a fixed script. The spectrum:

| Level | What the AI Does | Example |
|-------|-----------------|---------|
| **Completion** | Finishes your sentence | Copilot autocomplete |
| **Chat** | Answers your question | ChatGPT |
| **Tool use** | Calls tools when needed | Claude with MCP |
| **Agent** | Plans, executes, adapts | Claude Code full session |
| **Multi-agent** | Multiple agents collaborate | CrewAI, AutoGen |

## Core Patterns

### ReAct (Reason + Act)

The model alternates between reasoning and acting:

```
Thought: I need to find the bug in the test file
Action: Read tests/test_auth.py
Observation: [file contents]
Thought: The test expects a 200 but the endpoint returns 401 when...
Action: Read src/auth.py
Observation: [file contents]
Thought: Found it — the token validation skips expired check
Action: Edit src/auth.py (fix the bug)
```

This is what Claude Code does naturally.

### Planning

Before acting, create an explicit plan:

```
1. Read the failing test to understand expected behavior
2. Read the source file to find the bug
3. Write a fix
4. Run the test to verify
5. Commit
```

Claude Code's Plan mode (`/plan`) does this.

### Reflection

After acting, evaluate the result:

```
Action: Run tests
Result: 3 tests pass, 1 fails
Reflection: The fix broke test_token_refresh. Need to also update the refresh logic.
```

Self-review in Claude Code's subagent-driven-development pattern.

### Tool Use

The foundation — model outputs structured tool calls, host executes:

```json
{"tool": "read_file", "args": {"path": "src/auth.py"}}
```

### Subagent Dispatch

Spawn isolated agents for parallel or specialized work:
- Research agent: search codebase, web, docs
- Implementation agent: write code, run tests
- Review agent: check spec compliance, code quality

## Anti-Patterns

1. **Over-planning**: Planning for 30 minutes before writing 5 lines of code
2. **Tool abuse**: Calling tools when you already have the information
3. **Context pollution**: Dumping all search results into main context instead of using subagents
4. **Retry loops**: Same action, same inputs, expecting different results
5. **Premature multi-agent**: Using 3 agents when one tool call would suffice
