# Concept: LLM Wiki

An LLM wiki is a persistent, interlinked markdown knowledge base that sits between the user and the raw source corpus.

## Core Idea

- Raw sources remain immutable.
- The LLM reads those sources and maintains the wiki.
- The wiki stores summaries, entity pages, concept pages, comparisons, and syntheses.
- Useful answers are often saved back into the wiki as durable pages.

## Why It Matters

The point is not only retrieval. The point is to accumulate reusable synthesis so knowledge compounds over time.

## Architecture

- raw sources = source of truth
- wiki = maintained interpretation layer
- `AGENTS.md` = workflow/schema layer

## Related Pages

- [Overview](../overview.md)
- [Entity: Andrej Karpathy](../entities/andrej-karpathy.md)
- [Comparison: RAG vs LLM Wiki](../comparisons/rag-vs-llm-wiki.md)

## Source Basis

- [Source Summary: 2026-04-02 X Post](../sources/2026-04-02-x-post-summary.md)
- [Source Summary: 2026-04-04 `llm-wiki.md` Gist](../sources/2026-04-04-llm-wiki-gist-summary.md)
