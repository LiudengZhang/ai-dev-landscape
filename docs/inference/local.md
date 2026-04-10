# Local Inference

## Why Run Models Locally

1. **Cost**: Zero per-token cost after download
2. **Privacy**: Data never leaves your machine
3. **Latency**: No network round-trip for small models
4. **Offline**: Works without internet
5. **Experimentation**: Try different models freely

## The Stack

```
┌─────────────────────┐
│   Application       │  (your script, CLI tool)
├─────────────────────┤
│   Ollama / LM Studio│  (model management + API)
├─────────────────────┤
│   llama.cpp         │  (inference engine)
├─────────────────────┤
│   GGUF model file   │  (quantized weights)
├─────────────────────┤
│   Hardware           │  (CPU / GPU / Apple Silicon)
└─────────────────────┘
```

## Ollama

The easiest way to run models locally.

```bash
# Install
brew install ollama

# Run a model
ollama run qwen2.5:14b

# List installed models
ollama list

# Use in scripts (REST API)
curl http://localhost:11434/api/generate -d '{"model": "qwen2.5:14b", "prompt": "hello"}'

# Or via subprocess
ollama run qwen2.5:14b --nowordwrap <<< "your prompt here"
```

### Model Selection Guide

| Model | Size | Good For | Notes |
|-------|------|----------|-------|
| qwen2.5:7b | 4.7GB | Quick tasks, classification | Fast, fits on any Mac |
| qwen2.5:14b | 9GB | Issue screening, code review | My daily driver |
| qwen2.5:32b | 20GB | Complex reasoning | Needs 32GB+ RAM |
| llama3.1:8b | 4.7GB | General chat, summarization | Meta's standard |
| codellama:13b | 7.4GB | Code-specific tasks | Specialized for code |
| deepseek-coder-v2:16b | 8.9GB | Code generation | Strong on code |

### Performance on Apple Silicon

| Chip | RAM | Practical Max Model | Tokens/sec (14B) |
|------|-----|-------------------|-------------------|
| M1 | 16GB | 14B Q4 | ~15 |
| M1 Pro | 32GB | 32B Q4 | ~20 |
| M2 Pro | 32GB | 32B Q4 | ~25 |
| M3 Max | 64GB | 70B Q4 | ~20 |

## llama.cpp

The engine under Ollama. Use directly when you need:
- Custom quantization
- Specific sampling parameters
- Server mode with OpenAI-compatible API
- Batch processing

```bash
# Build from source
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp && make -j

# Run with specific parameters
./llama-cli -m model.gguf -p "prompt" --temp 0.0 -n 256
```

## My Setup

- **Hardware**: Apple Silicon Mac
- **Runtime**: Ollama
- **Daily model**: qwen2.5:14b (issue screening, code review triage)
- **Use case**: Pre-screening GitHub issues before spending Claude tokens on detailed analysis
