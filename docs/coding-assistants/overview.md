# Coding Assistants Overview

## The Landscape (2026)

Coding assistants have converged on two form factors: **terminal-native** (CLI agents) and **IDE-integrated** (editor extensions/forks). The key differentiator is no longer "can it write code?" but "how much autonomy do you give it?"

## Comparison

| Tool | Form Factor | Model | Autonomy | Open Source | Key Strength |
|------|------------|-------|----------|-------------|-------------|
| **Claude Code** | CLI | Claude | High (agentic) | No | MCP + skills + hooks ecosystem |
| **Codex CLI** | CLI | GPT | High (agentic) | Yes | Sandboxed execution, open source |
| **Cursor** | IDE (VS Code fork) | Multi | Medium-High | No | Codebase indexing, tab completion |
| **Copilot** | IDE extension | GPT | Low-Medium | No | Ubiquity, agent mode growing |
| **Windsurf** | IDE (fork) | Multi | High | No | Cascade multi-step flow |
| **Aider** | CLI | Multi | Medium | Yes | Git-aware, model-agnostic |
| **Continue** | IDE extension | Multi | Low-Medium | Yes | Model-agnostic, extensible |

## The Autonomy Spectrum

```
Low autonomy                                          High autonomy
├──────────┼──────────┼──────────┼──────────┼──────────┤
Copilot    Continue   Cursor    Aider      Claude Code
(suggest)  (chat)     (edit)    (multi-    (full agent
                                 file)     + tools)
```

**Low autonomy**: Suggests completions, you accept/reject line by line.
**High autonomy**: Plans multi-step changes, runs commands, creates files, uses external tools.

## What I Use

- **Claude Code** for everything. MCP servers extend its reach, skills codify my patterns, hooks automate my workflow.
- **Ollama + custom scripts** for cost-sensitive batch operations (issue screening).
- Previously tried Cursor — good for exploration, but terminal-native agentic flow is more powerful once you learn it.

## Key Trends

1. **CLI agents are winning** for power users. IDE integration still matters for discovery and onboarding.
2. **Model-agnostic tools** (Aider, Continue) give flexibility but lack deep integration.
3. **Autonomy is increasing** — the question is shifting from "can it do this?" to "should I let it?"
4. **Tool use > code generation** — the value is in agents that can read docs, run tests, check CI, not just write code.
