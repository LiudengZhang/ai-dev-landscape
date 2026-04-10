# RAG Architectures

## What Is RAG?

Retrieval-Augmented Generation: instead of relying only on the model's training data, **retrieve relevant documents at query time** and include them in the prompt.

```
User query
    ↓
Retrieve relevant chunks from a knowledge base
    ↓
Stuff retrieved chunks into prompt context
    ↓
Model generates answer grounded in retrieved text
```

## Why RAG?

- **Freshness**: Knowledge base can be updated without retraining
- **Grounding**: Reduces hallucination by providing source text
- **Domain specificity**: Add your own docs, papers, codebase
- **Cost**: Cheaper than fine-tuning for most use cases

## Architecture Variants

### Naive RAG

```
Query → Embed → Vector search → Top-K chunks → LLM → Answer
```

Simple, works surprisingly well for many cases. Fails when:
- Query and answer use different vocabulary
- Answer requires synthesizing across multiple documents
- Chunks lose context from surrounding text

### Graph RAG (LightRAG, Microsoft GraphRAG)

```
Documents → Extract entities + relationships → Build knowledge graph
Query → Graph traversal + vector search → Subgraph context → LLM → Answer
```

**LightRAG** (HKUDS): Lightweight graph-based RAG. Extracts entities and relations, stores in PostgreSQL/AGE graph + Qdrant vectors. Dual retrieval: graph traversal for structural queries, vector search for semantic queries.

**Key insight**: Graphs capture relationships between concepts that vector similarity misses.

### Agentic RAG

```
Query → Agent decides retrieval strategy → Multiple retrieval calls →
Agent synthesizes → May retrieve more → Answer
```

The agent decides *how* to retrieve, not just *what*. Can:
- Reformulate queries
- Retrieve from multiple sources
- Verify answers against sources
- Iterate until satisfied

### Hybrid Search

Combine vector (semantic) search with keyword (BM25) search:

| Method | Finds | Misses |
|--------|-------|--------|
| Vector search | Semantically similar | Exact terms, rare words |
| Keyword search | Exact matches | Paraphrased content |
| Hybrid | Both | Less than either alone |

## Chunking Strategies

How you split documents into retrievable units matters enormously:

| Strategy | How | Best For |
|----------|-----|----------|
| Fixed-size | Split every N tokens | Simple, predictable |
| Sentence | Split on sentence boundaries | Preserving meaning |
| Semantic | Split when topic changes | Long documents |
| Recursive | Split by headers, then paragraphs, then sentences | Structured documents |
| Parent-child | Retrieve child chunks, include parent for context | Maintaining context |

## My Experience

- Used LightRAG (investigated for OSS contribution) — graph extraction is powerful but adds complexity
- Ollama + manual prompting for simple retrieval tasks
- For code: embedding-based search (Cursor, Claude Code's Grep) works better than document RAG
