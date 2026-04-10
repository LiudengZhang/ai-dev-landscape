# Agent Framework Landscape

## What Is an Agent Framework?

A library that handles the **tool-calling loop**: model generates a tool call → framework executes it → result fed back to model → repeat until done.

The hard parts they solve:
- **State management**: Tracking conversation history, tool results, intermediate state
- **Tool orchestration**: Defining, registering, and executing tools
- **Error recovery**: Retrying, fallbacks, human-in-the-loop
- **Multi-agent coordination**: Multiple agents collaborating or competing

## The Landscape

### OpenClaw

- **What**: Open-source Claude Code alternative
- **Stack**: TypeScript, pnpm, vitest
- **Architecture**: Single-agent tool loop with context pruning
- **Strengths**: Full Claude Code feature parity goal, CJK support, active development
- **My experience**: Contributed 4 PRs — stale tool detection, CJK token estimation, symlink bootstrap

### LangGraph

- **What**: Stateful agent graphs from LangChain team
- **Stack**: Python, built on LangChain
- **Architecture**: Directed graphs with cycles, state machines for agent workflows
- **Strengths**: Persistence, human-in-the-loop checkpoints, complex multi-step workflows
- **When to use**: When your agent needs explicit state management and deterministic branching

### CrewAI

- **What**: Multi-agent orchestration with roles
- **Stack**: Python
- **Architecture**: Role-based agents with defined goals, backstory, tools
- **Strengths**: Simple mental model (agents as team members), easy to set up
- **Limitations**: Less flexible than LangGraph for complex flows

### AutoGen (Microsoft)

- **What**: Multi-agent conversation framework
- **Stack**: Python
- **Architecture**: Agents in group chats, conversational patterns
- **Strengths**: Group chat patterns, code execution, nested conversations
- **Limitations**: Complex API, heavy abstraction

### smolagents (Hugging Face)

- **What**: Lightweight code agents
- **Stack**: Python
- **Architecture**: Agents write Python code directly (not JSON tool calls)
- **Strengths**: Simple, transparent, agents write real code
- **Limitations**: Less structured than tool-calling approaches

### Pydantic AI

- **What**: Type-safe agent framework
- **Stack**: Python, Pydantic
- **Architecture**: Structured outputs + dependency injection
- **Strengths**: Type safety, Pydantic ecosystem, clean API
- **When to use**: When you want structured, validated outputs from agents

## Comparison Matrix

| Framework | Multi-Agent | State Mgmt | Tool Calling | Code Execution | Complexity |
|-----------|-----------|------------|-------------|---------------|------------|
| OpenClaw | No | Context window | JSON schema | Sandboxed bash | Medium |
| LangGraph | Yes | Graph state | LangChain tools | Via tools | High |
| CrewAI | Yes | Role-based | YAML/code | Via tools | Low |
| AutoGen | Yes | Chat history | Python | Built-in | High |
| smolagents | No | Minimal | Code-based | Direct Python | Low |
| Pydantic AI | No | DI container | Typed tools | Via tools | Medium |

## My Take

The framework wars are mostly about **abstraction level**. At the core, every framework does:

```
while not done:
    response = model.generate(messages, tools)
    if response.has_tool_call:
        result = execute_tool(response.tool_call)
        messages.append(result)
    else:
        done = True
```

The value is in the details: how they handle state, errors, concurrency, and multi-agent coordination. For simple single-agent use cases, you barely need a framework — a 50-line script works.
