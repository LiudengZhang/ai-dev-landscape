# Vector Databases

## What They Do

Store high-dimensional vectors (embeddings) and find the most similar ones efficiently. The retrieval engine behind RAG.

## How Similarity Search Works

1. **Embed** your documents into vectors (e.g., 768 or 1536 dimensions)
2. **Index** vectors for fast approximate nearest neighbor (ANN) search
3. **Query**: embed the query, find top-K most similar vectors
4. **Return** the associated documents/chunks

## Tools Compared

| Tool | Language | ANN Index | Persistence | Filtering | Best For |
|------|----------|-----------|-------------|-----------|----------|
| **Chroma** | Python | HNSW | SQLite | Metadata | Prototyping, small scale |
| **Qdrant** | Rust | HNSW | Disk/Memory | Payload filters | Production, filtering |
| **FAISS** | C++/Python | IVF, HNSW, PQ | Memory only | None (DIY) | Research, billion-scale |
| **Weaviate** | Go | HNSW | Disk | GraphQL | Full-featured, hybrid search |
| **Milvus** | Go/C++ | Multiple | Disk | Attribute | Enterprise, distributed |
| **Pinecone** | Managed | Proprietary | Cloud | Metadata | Serverless, zero-ops |
| **pgvector** | SQL | IVF, HNSW | PostgreSQL | SQL WHERE | When you already use Postgres |

## Key Concepts

### Embedding Models

| Model | Dimensions | Quality | Speed | Open Source |
|-------|-----------|---------|-------|-------------|
| OpenAI text-embedding-3-large | 3072 | High | API | No |
| Cohere embed-v4 | 1024 | High | API | No |
| BGE-large-en-v1.5 | 1024 | Good | Fast | Yes |
| nomic-embed-text | 768 | Good | Fast | Yes (Ollama) |
| all-MiniLM-L6-v2 | 384 | Decent | Very fast | Yes |

### HNSW (Hierarchical Navigable Small World)

The dominant ANN algorithm. Builds a multi-layer graph where:
- Top layers: sparse, for quick global navigation
- Bottom layers: dense, for precise local search
- Query traverses top-down, getting closer at each layer

Trade-offs: `ef_construction` (build quality vs speed), `M` (memory vs recall).

### Distance Metrics

| Metric | Formula | When to Use |
|--------|---------|-------------|
| Cosine similarity | 1 - cos(a,b) | Normalized embeddings (most common) |
| Euclidean (L2) | sqrt(sum((a-b)^2)) | Unnormalized embeddings |
| Dot product | sum(a*b) | When magnitude matters |

## My Setup

- **LightRAG uses**: Qdrant for vector storage, PostgreSQL/AGE for graph
- **For experiments**: Chroma (simplest to set up, good for prototyping)
- **Embedding**: nomic-embed-text via Ollama (local, free)
