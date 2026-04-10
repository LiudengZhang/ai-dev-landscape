# LLM Training Pipeline

## The Short Answer

Modern LLMs are built in stages: **pretraining** (predict next token on internet-scale text) → **supervised fine-tuning** (learn to follow instructions) → **alignment** (learn human preferences via RLHF or DPO).

## The Pipeline

### Stage 1: Pretraining

- Train on trillions of tokens (web, books, code)
- Objective: next-token prediction (autoregressive)
- This is where the model learns language, facts, and reasoning patterns
- Cost: millions of dollars, weeks of GPU time
- Output: a "base model" that can complete text but can't follow instructions well

### Stage 2: Supervised Fine-Tuning (SFT)

- Train on curated (instruction, response) pairs
- Teaches the model to follow instructions, answer questions, refuse harmful requests
- Much smaller dataset (tens of thousands of examples)
- Output: an "instruct model" that follows directions

### Stage 3: Alignment

**RLHF (Reinforcement Learning from Human Feedback):**
1. Collect human preference data (which response is better?)
2. Train a reward model on these preferences
3. Use PPO to optimize the LLM against the reward model

**DPO (Direct Preference Optimization):**
- Skip the reward model entirely
- Directly optimize the LLM on preference pairs
- Simpler, cheaper, increasingly preferred

## Key Concepts

| Concept | What It Means |
|---------|--------------|
| **Scaling laws** | More compute → predictably better performance (Kaplan et al., 2020) |
| **Chinchilla optimal** | For a given compute budget, there's an optimal model size vs data ratio (Hoffmann et al., 2022) |
| **Emergent abilities** | Capabilities that appear suddenly at scale (debated — may be metric artifacts) |
| **Mixture of Experts (MoE)** | Only activate a subset of parameters per token. Larger model, same inference cost |
| **Distillation** | Train a small model to mimic a large one. How Qwen/Llama small variants are made |

## What I Use

- **Ollama** for running distilled models locally (Qwen 2.5 14B)
- **Anthropic API** for Claude (full-scale, no local option)
- Understanding this pipeline helps evaluate model quality claims — "fine-tuned on X" means SFT, not pretraining
