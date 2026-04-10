# Scaling and Distillation

## Scaling Laws

The key insight from Kaplan et al. (2020): model performance is a **power law** function of compute, data, and parameters. Double compute → predictable improvement.

### Chinchilla Scaling (Hoffmann et al., 2022)

Previous approach (GPT-3 era): make models as big as possible.
Chinchilla showed: for a fixed compute budget, **train a smaller model on more data**.

| Model | Parameters | Training Tokens | Approach |
|-------|-----------|----------------|----------|
| GPT-3 | 175B | 300B | Over-parameterized |
| Chinchilla | 70B | 1.4T | Compute-optimal |
| Llama 3 | 70B | 15T | Over-trained (inference-optimal) |

**Current trend:** Over-train smaller models beyond Chinchilla-optimal. The extra training cost pays off because inference is cheaper with smaller models.

## Distillation

Compress knowledge from a large "teacher" model into a smaller "student" model.

### How It Works

1. Run teacher model on a large dataset, collect output distributions
2. Train student to match teacher's output distributions (not just hard labels)
3. Student learns the teacher's "dark knowledge" — which wrong answers are almost right

### Why It Matters

- Qwen 2.5 14B performs well because it's distilled from larger Qwen models
- DeepSeek-R1 distilled reasoning chains into smaller models
- This is how you get good local models on consumer hardware

## Quantization

Reduce model precision to save memory and speed up inference.

| Format | Bits | Quality | Use Case |
|--------|------|---------|----------|
| FP16 | 16 | Full | Training, high-end serving |
| INT8 | 8 | ~99% | Production serving |
| INT4 (GPTQ/AWQ) | 4 | ~95% | Consumer GPUs |
| GGUF Q4_K_M | ~4.5 | ~96% | CPU/Mac inference via llama.cpp |

**My setup:** Qwen 2.5 14B Q4_K_M via Ollama on M-series Mac — 9GB, fast enough for issue screening.
