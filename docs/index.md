# AI Dev Landscape

> The AI developer ecosystem moves weekly. This is my map — what exists, how it works, and what's worth adopting.

## What This Is

A personal field guide to the AI tooling landscape. Not a tutorial collection or link dump — a structured reference for understanding **how these tools work**, **how they relate**, and **what's worth investing time in**.

## The Stack

The AI developer ecosystem has layers, and understanding where a tool sits changes how you evaluate it:

| Layer | What It Does | Key Tools |
|-------|-------------|-----------|
| **Fundamentals** | How models are trained | Transformers, RLHF, DPO, distillation |
| **Inference** | Running models | Ollama, vLLM, llama.cpp, SGLang |
| **Coding Assistants** | AI writes code | Claude Code, Codex, Cursor, Copilot |
| **Agent Frameworks** | Autonomous tool use | OpenClaw, LangGraph, CrewAI, smolagents |
| **Protocols** | Integration standards | MCP, tool use, A2A |
| **RAG** | Knowledge retrieval | LightRAG, LlamaIndex, Chroma, Qdrant |
| **Patterns** | How to build with AI | Skills, hooks, agentic workflows |

## How to Use This Site

- **Looking up a specific tool?** Check the relevant section in the sidebar.
- **Want depth?** [Deep reads](deep-reads/index.md) have detailed analyses of key tools.
- **Tracking trends?** The [Trends](trends/2026-04.md) section is updated monthly.
- **Comparing options?** Category overview pages have comparison tables.

## Quick Links

- [Claude Code Deep Dive](coding-assistants/claude-code.md) — how skills, hooks, and MCP work together
- [MCP Protocol](protocols/mcp.md) — the standard for tool integration
- [Agent Framework Landscape](agent-frameworks/landscape.md) — what exists and how they differ
- [Local Inference](inference/local.md) — running models on your Mac
