# AI Dev Landscape

> The AI developer ecosystem moves weekly. This is my map — what exists, how it works, and what's worth adopting.

A personal field guide to the AI tooling landscape: from model training to coding assistants to agent frameworks. Structured for quick lookup and deep understanding.

## Contents

- [LLM Fundamentals](#llm-fundamentals)
- [Inference and Serving](#inference-and-serving)
- [Coding Assistants](#coding-assistants)
- [Agent Frameworks](#agent-frameworks)
- [Protocols and Standards](#protocols-and-standards)
- [RAG and Knowledge](#rag-and-knowledge)
- [AI-Native Dev Patterns](#ai-native-dev-patterns)
- [Trends and Signals](#trends-and-signals)
- [Companion Site](#companion-site)

## LLM Fundamentals

How models are built, trained, and improved.

- [Anthropic Research](https://www.anthropic.com/research) - Claude model family research and safety publications.
- [OpenAI Research](https://openai.com/research/) - GPT family, RLHF, scaling laws.
- [Hugging Face Transformers](https://github.com/huggingface/transformers) - The standard library for working with pretrained models (GitHub, 2018).
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) - Visual walkthrough of the transformer architecture (Blog, 2018).
- [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) - The paper that launched the scaling paradigm (arXiv, 2020).
- [RLHF — Illustrated](https://huggingface.co/blog/rlhf) - How reinforcement learning from human feedback works in practice (HF Blog, 2023).
- [Direct Preference Optimization (DPO)](https://arxiv.org/abs/2305.18290) - Simpler alternative to RLHF, no reward model needed (arXiv, 2023).
- [Distillation](https://arxiv.org/abs/1503.02531) - Compressing large models into smaller ones (arXiv, 2015).

## Inference and Serving

Running models locally and at scale.

- [Ollama](https://github.com/ollama/ollama) - Run LLMs locally with one command. Supports GGUF quantized models (GitHub, 2023).
- [vLLM](https://github.com/vllm-project/vllm) - High-throughput serving engine with PagedAttention for efficient KV cache (GitHub, 2023).
- [llama.cpp](https://github.com/ggml-org/llama.cpp) - CPU/GPU inference for GGUF models in pure C/C++. The engine behind Ollama (GitHub, 2023).
- [TGI (Text Generation Inference)](https://github.com/huggingface/text-generation-inference) - Hugging Face's production serving stack with continuous batching (GitHub, 2022).
- [SGLang](https://github.com/sgl-project/sglang) - Fast structured generation and serving with RadixAttention (GitHub, 2024).
- [ExLlamaV2](https://github.com/turboderp/exllamav2) - GPTQ/EXL2 quantized inference, fastest for consumer GPUs (GitHub, 2023).
- [OpenRouter](https://openrouter.ai/) - Unified API across 100+ models from different providers. Pay-per-token routing.

## Coding Assistants

AI tools that write, review, and understand code.

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) - Anthropic's agentic CLI. MCP servers, skills, hooks, subagent dispatch. Terminal-native (Anthropic, 2025).
- [Codex CLI](https://github.com/openai/codex) - OpenAI's open-source coding agent. Runs in terminal with sandboxed execution (GitHub, 2025).
- [Cursor](https://www.cursor.com/) - VS Code fork with deep AI integration. Tab completion, chat, codebase indexing (App, 2023).
- [GitHub Copilot](https://github.com/features/copilot) - Inline code suggestions in VS Code/JetBrains. Agent mode added 2025 (GitHub, 2021).
- [Windsurf](https://windsurf.com/) - Codeium's IDE with "Cascade" agentic flow — multi-step coding with tool use (App, 2024).
- [Aider](https://github.com/paul-gauthier/aider) - Terminal pair programmer. Git-aware, multi-file edits, works with any LLM API (GitHub, 2023).
- [Continue](https://github.com/continuedev/continue) - Open-source Copilot alternative. VS Code/JetBrains extension, model-agnostic (GitHub, 2023).

## Agent Frameworks

Building autonomous AI agents.

- [OpenClaw](https://github.com/openclaw/openclaw) - Open-source Claude Code alternative. TypeScript, tool loop, context pruning (GitHub, 2025).
- [LangGraph](https://github.com/langchain-ai/langgraph) - Stateful agent graphs with LangChain. Cycles, persistence, human-in-the-loop (GitHub, 2024).
- [CrewAI](https://github.com/crewAIInc/crewAI) - Multi-agent orchestration with role-based agents. Simple API (GitHub, 2024).
- [AutoGen](https://github.com/microsoft/autogen) - Microsoft's multi-agent conversation framework. Supports group chat patterns (GitHub, 2023).
- [smolagents](https://github.com/huggingface/smolagents) - Hugging Face's lightweight agent library. Code agents that write Python directly (GitHub, 2025).
- [Pydantic AI](https://github.com/pydantic/pydantic-ai) - Type-safe agent framework built on Pydantic. Structured outputs, dependency injection (GitHub, 2024).
- [Claude Agent SDK](https://docs.anthropic.com/en/docs/agent-sdk) - Anthropic's SDK for building custom agents with Claude (Anthropic, 2025).

## Protocols and Standards

How AI tools communicate and integrate.

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) - Open standard for connecting AI assistants to external tools and data sources. JSON-RPC over stdio/SSE (Anthropic, 2024).
- [Tool Use / Function Calling](https://docs.anthropic.com/en/docs/build-with-claude/tool-use) - The pattern: model outputs structured tool calls, host executes, returns results. Universal across providers.
- [Agent-to-Agent (A2A)](https://github.com/google/A2A) - Google's protocol for agent interoperability. Complements MCP (GitHub, 2025).
- [OpenAPI / Swagger](https://swagger.io/specification/) - REST API description format. Many MCP servers auto-generate from OpenAPI specs.
- [Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs) - Constrained JSON generation from LLMs. JSON schema enforcement at decode time.

## RAG and Knowledge

Retrieval-augmented generation and knowledge management.

- [LightRAG](https://github.com/HKUDS/LightRAG) - Graph-based RAG with knowledge graph extraction. PostgreSQL/AGE + vector DB hybrid (GitHub, 2024).
- [RAG-Anything](https://github.com/HKUDS/RAG-Anything) - Multimodal RAG for documents with mixed content types (GitHub, 2025).
- [LlamaIndex](https://github.com/run-llama/llama_index) - Data framework for LLM apps. Connectors, indexes, query engines (GitHub, 2022).
- [Chroma](https://github.com/chroma-core/chroma) - Embedding database. Simple API, runs in-process or client-server (GitHub, 2022).
- [Qdrant](https://github.com/qdrant/qdrant) - Vector similarity search engine in Rust. Filtering, payload storage (GitHub, 2021).
- [FAISS](https://github.com/facebookresearch/faiss) - Facebook's similarity search library. GPU-accelerated, billion-scale (GitHub, 2017).
- [Unstructured](https://github.com/Unstructured-IO/unstructured) - ETL for documents — PDF, HTML, images to clean text chunks (GitHub, 2022).

## AI-Native Dev Patterns

Patterns and practices for building with AI tools.

- [MCP Servers](https://github.com/modelcontextprotocol/servers) - Official + community MCP server implementations. Reference patterns for building your own (GitHub, 2024).
- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills) - Reusable prompt templates that extend Claude Code's capabilities. Markdown files with structured instructions.
- [Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks) - Shell commands triggered by Claude Code events. Pre/post tool execution, notifications.
- [Prompt Engineering Guide](https://www.promptingguide.ai/) - Comprehensive guide to prompting techniques: CoT, few-shot, ReAct, etc.
- [Cursor Rules](https://cursor.directory/) - Community-shared `.cursorrules` files for project-specific AI behavior.
- [Agentic Design Patterns](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/) - Andrew Ng's overview: reflection, tool use, planning, multi-agent (Blog, 2024).

## Trends and Signals

What's moving right now. Updated periodically.

### 2026-04 (Current)

- **Coding agents converge**: Claude Code, Codex CLI, Copilot agent mode all shipping. The terminal-vs-IDE split is the main differentiator.
- **MCP adoption accelerates**: Anthropic's protocol becoming cross-vendor. Cursor, Windsurf, Continue all support MCP servers.
- **Local models get practical**: Qwen 2.5 14B runs well on M-series Macs via Ollama. Good enough for screening/triage tasks.
- **Agent frameworks proliferate**: LangGraph, CrewAI, smolagents, Pydantic AI — no clear winner yet. Most are thin wrappers over tool-calling loops.
- **A2A vs MCP**: Google's Agent-to-Agent protocol launched. Positioned as complementary to MCP (agents talk to agents vs agents talk to tools).

## Companion Site

The [companion site](https://liudengzhang.github.io/ai-dev-landscape/) goes deeper with:

- **Deep reads** on key tools (how Claude Code works internally, MCP protocol walkthrough)
- **Comparisons** (agent frameworks head-to-head, local model benchmarks)
- **Trends log** (monthly updates on notable repos, papers, paradigm shifts)
