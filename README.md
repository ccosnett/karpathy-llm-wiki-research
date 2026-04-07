# Karpathy LLM Wiki Research

This repository is a compact research note on Andrej Karpathy's April 2026 idea around "LLM Knowledge Bases" / "LLM Wiki" style agent-maintained knowledge systems.

It is a snapshot written on 2026-04-07. It focuses on what Karpathy appears to have actually proposed, the tooling he explicitly mentioned, and the early public ecosystem that formed around the idea.

## The Short Version

Karpathy's proposal is not "better RAG" in the narrow sense. It is closer to a compiled knowledge layer for agents:

- Raw source documents stay immutable.
- An LLM agent reads those sources and continuously writes a persistent, interlinked markdown wiki.
- The user curates sources and asks questions.
- The LLM does the bookkeeping: summaries, cross-links, updates, contradiction tracking, synthesis, and maintenance.

His core claim is that ordinary RAG re-discovers knowledge every time you ask a question, while a wiki maintained by an LLM compounds knowledge over time into a reusable artifact.

## Timeline

- 2026-04-02: Karpathy posted on X about "LLM Knowledge Bases" and described using LLMs to build personal knowledge bases instead of spending most of his token budget only on code. Source: [x.com/karpathy/status/2039805659525644595](https://x.com/karpathy/status/2039805659525644595)
- 2026-04-04: Karpathy published the more complete idea file gist `llm-wiki.md`. Source: [gist.github.com/karpathy/442a6bf555914893e9891c11519de94f](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- 2026-04-05 to 2026-04-07: early public repos and clones started appearing that explicitly cite the pattern or adapt it to domain-specific use cases.

## What Karpathy Is Actually Proposing

From the gist, the proposal has a few stable ingredients:

- A persistent wiki sits between the human and the raw corpus.
- The wiki is markdown-first and usually git-backed.
- The LLM, not the human, is the main author and maintainer of the wiki.
- The human stays in the loop for curation, emphasis, and quality control.
- The wiki is not just for passive retrieval. It is meant to absorb query outputs and new syntheses back into itself.
- The system is configured by a schema file such as `AGENTS.md` or `CLAUDE.md` that tells the LLM how to maintain the wiki.

The best one-line summary from Karpathy's framing is this:

- RAG answers from documents.
- An LLM wiki compiles documents into a maintained knowledge artifact.

## Why He Thinks This Beats Plain RAG

Karpathy's argument is roughly:

- Plain RAG does retrieval-time reconstruction over and over.
- Harder questions often require combining multiple documents, entities, and contradictions each time from scratch.
- A wiki lets the expensive synthesis happen once, then get refined incrementally.
- The accumulated artifact becomes denser and more useful after every source ingest and after every meaningful question.

This makes the knowledge base "compounding" rather than stateless.

## Core Architecture

Karpathy describes three layers:

1. Raw sources
   Immutable source material such as papers, articles, notes, images, and data files.
2. The wiki
   A directory of LLM-generated markdown pages: summaries, entities, concepts, comparisons, overviews, and syntheses.
3. The schema
   A workflow/config document that tells the LLM how to ingest, organize, answer, update, and lint the wiki.

This is important because the wiki is not the source of truth. The raw documents remain the source of truth. The wiki is the maintained interpretation layer.

## Core Operations

Karpathy's gist breaks the system into a few recurring operations:

- Ingest
  Add a source, have the LLM read it, summarize it, update relevant entity/concept pages, update the index, and log what changed.
- Query
  Ask questions against the wiki, then file the resulting analysis back into the wiki if it is useful.
- Lint
  Ask the LLM to look for contradictions, stale claims, orphan pages, missing links, missing concept pages, and obvious research gaps.

This is one of the more important ideas in the whole pattern: answers are not disposable chat output. Good answers become new durable wiki pages.

## Special Files Karpathy Calls Out

- `index.md`
  A content-oriented catalog of the wiki, used to navigate pages and often sufficient at moderate scale without embeddings.
- `log.md`
  A chronological append-only record of ingests, queries, and lint passes.
- `AGENTS.md` / `CLAUDE.md`
  The operating manual for the agent so it behaves like a disciplined maintainer instead of a generic chatbot.

## Tooling Karpathy Explicitly Mentions

These are mentioned directly in the gist, so they are part of the public shape of the idea:

- [Obsidian](https://obsidian.md/)
  Karpathy describes using the LLM agent on one side and Obsidian on the other, with Obsidian acting like the "IDE" for the wiki.
- [Obsidian Web Clipper](https://obsidian.md/clipper)
  A practical way to turn web articles into markdown sources.
- [qmd](https://github.com/tobi/qmd)
  Mentioned as a local search option once the wiki grows beyond what `index.md` can comfortably support.
- [Marp](https://marp.app/)
  Suggested for generating slide decks from wiki content.
- [Dataview](https://github.com/blacksmithgu/obsidian-dataview)
  Useful if the LLM writes YAML frontmatter and you want dynamic tables/lists inside Obsidian.
- [git](https://git-scm.com/)
  Karpathy explicitly notes that the wiki being "just a git repo of markdown files" gives versioning, history, and collaboration for free.

## Use Cases Karpathy Explicitly Names

- Personal self-tracking and self-understanding.
- Deep topic research over weeks or months.
- Reading books and building companion/fan-wiki-like structures.
- Business or team internal wikis from meetings, Slack, customer calls, and project docs.
- Competitive analysis, due diligence, trip planning, course notes, and hobby deep-dives.

The pattern is intentionally domain-agnostic.

## What Makes This Feel New

The distinctive part is not "store documents and query them later." That is already common.

The novel emphasis is:

- persistent markdown artifacts instead of ephemeral chat outputs
- an LLM as wiki maintainer, not only question-answerer
- cross-reference maintenance as a first-class job
- structured synthesis that compounds over time
- treating agent outputs as things worth filing back into the knowledge base

In other words, this is closer to agent-maintained memory engineering than to classic document chat.

## My Practical Interpretation

If you strip away the specific tools, the pattern looks like this:

- Use raw files as canonical evidence.
- Maintain a compiled semantic layer in markdown.
- Teach the agent explicit maintenance workflows.
- Keep humans responsible for curation and judgment.
- Let the agent do the repetitive editorial work.

That makes "LLM Wiki" a useful design pattern for agent memory, research systems, due-diligence agents, internal company knowledge bases, and personal intelligence systems.

## Where This Seems Strong

- It creates reusable structure instead of throwing away work inside chat history.
- It fits well with existing developer tooling because markdown, git, grep, editors, and search already exist.
- It gives the LLM a tangible working memory substrate across sessions.
- It supports inspection by humans because the artifact is readable and editable.
- It does not require a heavyweight vector database on day one.

## Where The Risks Are

Karpathy's gist is deliberately high level, so many hard details are left open:

- Citation discipline and provenance can still drift if the schema is weak.
- The wiki can solidify wrong interpretations if the ingest workflow is sloppy.
- Contradiction handling is only as good as the prompts, tooling, and review loop.
- Scaling beyond "moderate" size likely requires stronger search/indexing than a single markdown index file.
- Multi-user editing, permissions, and review workflows are not really specified.
- Sensitive corpora raise privacy and governance questions.
- Different agents may update the wiki differently unless the schema is tightly controlled.

So the idea is strong as a pattern, but not complete as a production specification.

## Relationship To Older Ideas

Karpathy explicitly relates the idea to Vannevar Bush's 1945 Memex concept: a personal curated knowledge system where associative links between documents matter. His claim is that LLMs finally make the maintenance cost low enough for this kind of system to stay alive instead of decaying.

## Early Ecosystem Around The Idea

Within days of the gist, public repos started appearing that adapt the pattern into skills, compilers, or domain-specific vaults. Examples visible on GitHub by 2026-04-07 include:

- [atomicmemory/llm-wiki-compiler](https://github.com/atomicmemory/llm-wiki-compiler)
  Markets itself as "The knowledge compiler" and explicitly says it is inspired by Karpathy's LLM Wiki pattern.
- [ussumant/llm-wiki-compiler](https://github.com/ussumant/llm-wiki-compiler)
  Frames itself as a Claude Code plugin implementing Karpathy's pattern.
- [xoai/sage-wiki](https://github.com/xoai/sage-wiki)
  Positions itself as an LLM-compiled personal knowledge base.
- [rvk7895/llm-knowledge-bases](https://github.com/rvk7895/llm-knowledge-bases)
  Describes itself as a Claude Code plugin for LLM-maintained Obsidian wikis.
- [colin4k/obsidian-wiki-compiler](https://github.com/colin4k/obsidian-wiki-compiler)
  Focuses on the Obsidian/plugin angle.
- [balukosuri/llm-wiki-karpathy](https://github.com/balukosuri/llm-wiki-karpathy)
  A direct public repo named after the pattern.
- [tkisdroid/real-estate-wiki](https://github.com/tkisdroid/real-estate-wiki)
  A domain-specific adaptation applying the pattern to real-estate exam study material.

These are not Karpathy's official implementation. They are evidence that the idea propagated quickly and that people immediately interpreted it as:

- an agent skill
- a knowledge compiler
- an Obsidian workflow
- a persistent memory system
- a domain-specific wiki generator

## The Best Mental Model

The cleanest mental model I have for this is:

- raw corpus = evidence
- wiki = compiled understanding
- schema = agent operating system
- log/index/search = support infrastructure
- human = curator and reviewer
- LLM = tireless editor and maintainer

That is why "agent knowledge base," "agent wiki," "LLM wiki," and "LLM knowledge base" all point at roughly the same concept here.

## Bottom Line

Karpathy's idea is a simple but powerful shift:

- stop treating agent outputs as disposable chat
- let the agent maintain a durable markdown knowledge layer
- keep that layer interlinked, inspectable, and under version control
- continuously fold new evidence and new analysis back into it

If the pattern works in practice, it gives agents something closer to a living, inspectable long-term memory than standard file-upload chat or naive RAG.

## Sources

- Andrej Karpathy, X post, 2026-04-02: [https://x.com/karpathy/status/2039805659525644595](https://x.com/karpathy/status/2039805659525644595)
- Andrej Karpathy, `llm-wiki.md` gist, 2026-04-04: [https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- Obsidian: [https://obsidian.md/](https://obsidian.md/)
- Obsidian Web Clipper: [https://obsidian.md/clipper](https://obsidian.md/clipper)
- qmd: [https://github.com/tobi/qmd](https://github.com/tobi/qmd)
- Marp: [https://marp.app/](https://marp.app/)
- Dataview: [https://github.com/blacksmithgu/obsidian-dataview](https://github.com/blacksmithgu/obsidian-dataview)
- git: [https://git-scm.com/](https://git-scm.com/)
- atomicmemory/llm-wiki-compiler: [https://github.com/atomicmemory/llm-wiki-compiler](https://github.com/atomicmemory/llm-wiki-compiler)
- ussumant/llm-wiki-compiler: [https://github.com/ussumant/llm-wiki-compiler](https://github.com/ussumant/llm-wiki-compiler)
- xoai/sage-wiki: [https://github.com/xoai/sage-wiki](https://github.com/xoai/sage-wiki)
- rvk7895/llm-knowledge-bases: [https://github.com/rvk7895/llm-knowledge-bases](https://github.com/rvk7895/llm-knowledge-bases)
- colin4k/obsidian-wiki-compiler: [https://github.com/colin4k/obsidian-wiki-compiler](https://github.com/colin4k/obsidian-wiki-compiler)
- balukosuri/llm-wiki-karpathy: [https://github.com/balukosuri/llm-wiki-karpathy](https://github.com/balukosuri/llm-wiki-karpathy)
- tkisdroid/real-estate-wiki: [https://github.com/tkisdroid/real-estate-wiki](https://github.com/tkisdroid/real-estate-wiki)
