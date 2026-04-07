# Source Summary: 2026-04-04 `llm-wiki.md` Gist

## Summary

This is the main architecture source for the example. It defines the pattern as a markdown wiki maintained by an LLM, backed by immutable raw sources and governed by a schema file.

## Key Points

- The wiki is a persistent, compounding artifact.
- The LLM writes and maintains the wiki.
- The system has three layers: raw sources, wiki, schema.
- The main operations are ingest, query, and lint.
- `index.md` and `log.md` are special support files.

## Derived Pages

- [Overview](../overview.md)
- [Entity: Andrej Karpathy](../entities/andrej-karpathy.md)
- [Concept: LLM Wiki](../concepts/llm-wiki.md)
- [Comparison: RAG vs LLM Wiki](../comparisons/rag-vs-llm-wiki.md)

## Raw Source

- [2026-04-04 `llm-wiki.md` gist note](../../raw/2026-04-04-llm-wiki-gist-note.md)
