# AGENTS.md

This is a minimal instantiation of Andrej Karpathy's LLM wiki pattern.

## Goal

Maintain a tiny wiki about Karpathy's own "LLM Wiki" / "LLM Knowledge Bases" idea.

## Layers

- `raw/`
  Immutable source material. This is the source of truth.
- `wiki/`
  LLM-maintained markdown knowledge base.
- `AGENTS.md`
  The schema and workflow instructions for the agent.

## Wiki Conventions

- `wiki/index.md` is the content-oriented catalog of the wiki.
- `wiki/log.md` is the chronological append-only record.
- Put entity pages in `wiki/entities/`.
- Put concept pages in `wiki/concepts/`.
- Put source-summary pages in `wiki/sources/`.
- Put comparison/analysis pages in `wiki/comparisons/`.
- Keep pages short, linked, and citation-aware.

## Hard Rules

- Never edit files in `raw/` unless the human explicitly asks.
- Prefer editing existing wiki pages over creating duplicates.
- Every factual statement in the wiki should be supportable by one or more files in `raw/`.
- Update `wiki/index.md` whenever you add a page.
- Append an entry to `wiki/log.md` after every ingest, query page creation, or lint pass.

## Ingest Workflow

1. Read the relevant files in `raw/`.
2. Create or update a source-summary page in `wiki/sources/`.
3. Update affected entity, concept, comparison, overview, or synthesis pages.
4. Update `wiki/index.md`.
5. Append an entry to `wiki/log.md` using a consistent heading format.

## Query Workflow

1. Read `wiki/index.md` first.
2. Open the most relevant wiki pages.
3. Answer with citations back to the source-summary pages and, where needed, to `raw/`.
4. If the answer is reusable, save it as a new or updated wiki page.
5. Update `wiki/index.md`.
6. Append an entry to `wiki/log.md`.

## Lint Workflow

Look for:

- contradictions between pages
- stale claims
- orphan pages with no inbound links
- concepts mentioned without their own page
- missing cross-references
- gaps that should trigger more sourcing
