---
name: rag-design
description: Design a Retrieval-Augmented Generation (RAG) pipeline — chunking strategy, embeddings, retrieval, reranking, and prompt assembly
---

You are designing a RAG (Retrieval-Augmented Generation) pipeline.

**Step 1 — Understand the use case**

Ask for (or extract):
1. What is the source data? (PDFs, web pages, database records, code files, Slack/email)
2. How large is the corpus? (hundreds of docs / millions of chunks)
3. What queries will users ask? (factual lookup, summarization, multi-hop reasoning)
4. What latency is acceptable? (real-time <500ms, batch, async)
5. What tech stack? (Python/LangChain, TypeScript, cloud provider preference)
6. Budget/scale constraints?

**Step 2 — Design each pipeline stage**

**A. Ingestion & Chunking**

| Strategy | When to use |
|---|---|
| Fixed-size (512 tokens, 20% overlap) | General purpose, mixed content |
| Sentence/paragraph | Structured prose, Q&A docs |
| Semantic (split on topic shift) | Long documents with clear sections |
| Hierarchical (page → section → chunk) | PDFs, reports — enables parent retrieval |
| Code-aware (function/class boundaries) | Source code corpora |

Overlap: 10-20% of chunk size. Too little = context breaks at boundaries. Too much = duplicate retrieval.

**B. Embeddings**

| Model | Best for |
|---|---|
| `text-embedding-3-large` (OpenAI) | General, high quality, 3072-dim |
| `text-embedding-3-small` | Cost-sensitive, still strong |
| `voyage-2` | Long documents, legal/technical |
| `nomic-embed-text` | Open-source, self-hosted |

Embed both documents and queries with the same model. Store normalized vectors.

**C. Vector Store**

| Store | When |
|---|---|
| Pinecone | Managed, scale, simple ops |
| pgvector (Postgres) | Already on Postgres, <10M vectors |
| Weaviate | Hybrid search (vector + BM25) built-in |
| Chroma | Local dev / small scale |
| Qdrant | High-performance, self-hosted |

**D. Retrieval**

- **Dense retrieval**: cosine similarity on embeddings — good for semantic queries
- **Sparse (BM25)**: keyword matching — good for exact terms, product names, codes
- **Hybrid**: combine both with RRF (Reciprocal Rank Fusion) — best overall

Top-K: start with K=10-20; filter down with reranker.

**E. Reranking**

Use a cross-encoder reranker after initial retrieval to reorder results by true relevance:
- `cross-encoder/ms-marco-MiniLM-L-6-v2` (fast, good)
- Cohere Rerank API (managed, strong)
- Retrieve K=20, rerank to top 5

**F. Prompt Assembly**

```
System: You are [role]. Answer using only the provided context.
        If the context does not contain the answer, say so explicitly.

Context:
[CHUNK 1 — source: {doc_title}, page {N}]
{chunk_text}

[CHUNK 2 — source: {doc_title}, page {N}]
{chunk_text}

...

User: {user_query}
```

Always include source attribution in chunks. Use `<context>` XML tags if the model supports it.

**Step 3 — Output format**

```
RAG PIPELINE DESIGN — [Use Case]
─────────────────────────────────────────────────────────────
PIPELINE OVERVIEW
  Corpus:      [size / format]
  Query type:  [factual / summarization / multi-hop]
  Latency SLA: [Xms]

STAGE RECOMMENDATIONS

  Chunking:    [strategy] | Chunk size: [N] tokens | Overlap: [N] tokens
  Embeddings:  [model] | Dimensions: [N] | Normalize: yes
  Vector store:[store] | Index type: [HNSW / IVF] | Metric: cosine
  Retrieval:   [dense / sparse / hybrid] | Initial K: [N]
  Reranker:    [model or none] | Final K: [N]
  LLM:         [model] | Max context: [N] tokens | Temp: 0

ESTIMATED COSTS (rough)
  Ingestion (one-time):  ~$[X] for [N] docs
  Per query:             ~$[X] (embedding + LLM)

FAILURE MODES TO MITIGATE
  □ Chunk boundary splits key info — add overlap and test with real queries
  □ No relevant chunk retrieved — add hybrid search + query expansion
  □ Model hallucinates beyond context — add "only use provided context" instruction + citation check
  □ Stale index — set up incremental upsert pipeline on doc changes

IMPLEMENTATION STACK
  [Recommended libraries and rough code structure]
```
