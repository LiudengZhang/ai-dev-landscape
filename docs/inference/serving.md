# Serving at Scale

## When Local Isn't Enough

Local inference works for personal use. For production or team use, you need:
- **Concurrent requests**: Multiple users hitting the same model
- **High throughput**: Batch processing thousands of requests
- **Low latency**: Sub-second response times
- **GPU efficiency**: Maximizing expensive hardware utilization

## Key Concepts

### Continuous Batching

Instead of processing one request at a time, batch multiple requests and process them together. As requests finish, new ones slot in immediately.

### KV Cache Management

During generation, attention key-value pairs are cached. This cache grows with sequence length and batch size — it's often the memory bottleneck.

**PagedAttention (vLLM)**: Manages KV cache like OS virtual memory — pages of cache can be non-contiguous, reducing fragmentation from ~60% to ~4%.

### Speculative Decoding

Use a small "draft" model to generate candidate tokens, then verify with the large model in parallel. Speeds up inference 2-3x when the draft model is accurate.

## Tools Compared

| Tool | Key Innovation | Best For |
|------|---------------|----------|
| **vLLM** | PagedAttention | High-throughput serving, production |
| **TGI** | Continuous batching + HF integration | HF ecosystem, easy deployment |
| **SGLang** | RadixAttention + structured output | Structured generation, multi-turn |
| **TensorRT-LLM** | NVIDIA kernel optimization | Maximum perf on NVIDIA GPUs |

## API Providers

When you don't want to manage infrastructure:

| Provider | Models | Pricing Model | Notes |
|----------|--------|---------------|-------|
| **Anthropic** | Claude family | Per-token | Best for complex reasoning |
| **OpenAI** | GPT family | Per-token | Broadest ecosystem |
| **OpenRouter** | 100+ models | Per-token, routing | Unified API, model switching |
| **Together AI** | Open models | Per-token | Good for fine-tuned open models |
| **Fireworks AI** | Open models | Per-token | Fast inference, competitive pricing |
