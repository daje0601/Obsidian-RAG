# What is RAG? (Retrieval-Augmented Generation)

> This is an example raw file. Try telling Claude Code "compile" to process this into the wiki!

## Overview

RAG (Retrieval-Augmented Generation) is a technique that enhances LLM responses by first retrieving relevant information from an external knowledge base, then using that context to generate more accurate answers.

## How Traditional RAG Works

1. **Ingestion**: Documents are split into chunks and converted to vector embeddings
2. **Storage**: Embeddings are stored in a vector database (Pinecone, Weaviate, ChromaDB, etc.)
3. **Retrieval**: When a user asks a question, the query is embedded and similar chunks are found via cosine similarity
4. **Generation**: Retrieved chunks are passed to the LLM as context to generate an answer

## Limitations of Traditional RAG

- Requires infrastructure (vector DB, embedding model, API)
- Chunk boundaries can split important context
- Embedding similarity doesn't always capture semantic relevance
- "Black box" — you can't easily browse or audit what's in the vector store
- Maintenance overhead: re-indexing when documents change

## Obsidian RAG Alternative

Andrej Karpathy proposed a simpler approach:
- Use structured markdown files instead of vector embeddings
- LLM navigates via hierarchical indexes (master index → topic index → article)
- Human-readable, auditable, and self-improving
- No infrastructure needed — just files and an LLM with file access

## Key Concepts

- **Embeddings**: Dense vector representations of text for similarity search
- **Vector Database**: Specialized DB for storing and querying embeddings
- **Cosine Similarity**: Measure of how similar two vectors are
- **Chunking**: Splitting documents into smaller pieces for embedding
- **Context Window**: Maximum text length an LLM can process at once
