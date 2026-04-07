# Raw Source: 2026-04-04 `llm-wiki.md` Gist Note

Type: human-curated source note
Status: immutable

## Summary

Karpathy describes a three-layer architecture:

- raw sources as immutable source of truth
- a markdown wiki maintained by the LLM
- a schema file such as `AGENTS.md` or `CLAUDE.md` that tells the LLM how to operate

He also describes three recurring operations:

- ingest
- query
- lint

## Claims Captured

- The wiki is a persistent, compounding artifact.
- The LLM writes and maintains the wiki.
- `index.md` is a content-oriented catalog.
- `log.md` is a chronological append-only record.
- Good query outputs should often be saved back into the wiki.
