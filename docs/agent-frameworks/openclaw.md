# OpenClaw Internals

## What I Learned From Contributing

I submitted 4 PRs to OpenClaw, which gave me a view into how a coding agent works internally.

## Architecture

OpenClaw is a TypeScript implementation of the agentic coding loop:

```
User input
    ↓
Prompt assembly (system prompt + context + user message)
    ↓
Model API call (Claude/other)
    ↓
┌─→ Parse response
│   ↓
│   Tool call? ──No──→ Display response, wait for input
│   ↓ Yes
│   Execute tool (file read, write, bash, etc.)
│   ↓
│   Append result to context
│   ↓
│   Context too long? ──Yes──→ Prune context
│   ↓ No
└───┘
```

## Key Components

### Context Pruning

The context window fills up during long sessions. OpenClaw prunes by:
1. Keeping system prompt and recent messages
2. Summarizing or dropping older tool results
3. CJK token estimation (my PR #40216) — CJK characters use more tokens than ASCII

### Tool Loop Detection

When the model gets stuck in a loop (calling the same tool repeatedly with the same args), the agent needs to detect and break the cycle. My PR #40217 added this detection.

### Bootstrap / Init

Setting up the agent environment — reading config, connecting to APIs, preparing the tool registry. My PR #40230 fixed symlink handling in the bootstrap files.

## Lessons

1. **Context management is the hard problem**: The tool loop is simple. Managing what fits in context is where real engineering happens.
2. **Token counting is approximate**: Different models tokenize differently. CJK text throws off byte-based estimates.
3. **Greptile bot**: OpenClaw uses a Greptile bot for automated PR review. Fix its issues before the maintainer sees your PR.
