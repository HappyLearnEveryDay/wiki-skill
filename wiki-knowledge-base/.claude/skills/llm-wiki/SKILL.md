---
name: llm-wiki
description: Build and maintain a persistent, LLM-authored wiki as a personal or team knowledge base. Use when the user wants to create a knowledge base, wiki, or second brain from articles/papers/notes, asks to ingest a source into a wiki, wants to query or synthesize across accumulated knowledge, or mentions LLM wikis, compounding notes, Memex-style systems, or alternatives to RAG for personal knowledge.
---

# LLM Wiki

A pattern for building **persistent, compounding knowledge bases** using an LLM as the maintainer. Instead of retrieving raw documents at query time (RAG), the LLM incrementally compiles sources into a structured, interlinked wiki of markdown files that keeps getting richer with every source added.

**Core insight**: the expensive cognitive work (summarizing, cross-referencing, flagging contradictions, synthesizing) happens once at ingest time and is persisted. Queries reuse that work instead of redoing it.

## When to use this skill

Apply this skill when the user wants to:
- Start a new personal / research / reading / team wiki
- Ingest an article, paper, chapter, transcript, or note into an existing wiki
- Query across an existing wiki (and optionally file the answer back)
- Lint / health-check a wiki (orphan pages, contradictions, stale claims)
- Evolve the wiki's schema (conventions, page types, workflows)

If there is no existing wiki in the working directory, start with **Initialize a new wiki**.
If there is an existing wiki, read its `AGENTS.md` (or `CLAUDE.md`) first — it overrides any default in this skill.

## Three-layer architecture

Every wiki has three layers. Keep them strictly separated.

1. **Raw sources** (`raw/`) — the original documents. Immutable. The LLM reads but never edits.
2. **Wiki** (`wiki/` or repo root) — LLM-authored markdown. Entity pages, concept pages, source summaries, syntheses, index, log. The LLM owns this entirely.
3. **Schema** (`AGENTS.md` in the wiki root) — the conventions document. Directory layout, naming rules, frontmatter fields, workflows, lint rules. Co-evolved with the user.

## Default directory structure

Use this as the default when initializing. Allow the user to adjust.

```
<wiki-root>/
├── AGENTS.md              # schema; the operating manual for the LLM
├── index.md               # content catalog, updated on every ingest
├── log.md                 # chronological record, append-only
├── overview.md            # top-level synthesis / entry page
├── sources/               # one summary page per ingested source
├── entities/              # people, products, orgs, places, works
├── concepts/              # methods, theories, patterns, abstractions
├── syntheses/             # comparisons, analyses, LLM-generated insights
├── questions/             # open questions, hypotheses to test (optional)
└── raw/                   # immutable source files
    ├── articles/
    └── assets/            # images, PDFs referenced by sources
```

The separation between `sources/`, `entities/`, `concepts/`, `syntheses/` is load-bearing — these page types have different lifecycles. Do not collapse them into a single flat folder.

## Page types and templates

See `page-templates.md` for full frontmatter and structure for each page type. Summary:

| Type | Lifecycle | Contents |
|------|-----------|----------|
| `sources/*.md` | Written once at ingest | TL;DR, key claims, citations, pages touched |
| `entities/*.md` | Grows with sources | Objective facts about a named thing, with source citations |
| `concepts/*.md` | Grows with sources | Definitions, mechanisms, relationships to other concepts |
| `syntheses/*.md` | Written on user queries | Comparisons, analyses, opinions — with clear attribution |
| `questions/*.md` | Written as they arise | Open questions, hypotheses, things to research next |
| `index.md` | Updated every ingest | Catalog of all pages, grouped by type |
| `log.md` | Append-only | `## [YYYY-MM-DD] action \| title` entries |
| `overview.md` | Periodically refreshed | High-level map of the wiki |

## Core operations

See `workflows.md` for detailed step-by-step procedures. Overview:

### Ingest

User drops a new source and asks to ingest it. Flow:

1. Read the source from `raw/`
2. Discuss key takeaways with the user (brief — don't dump the whole article)
3. Write `sources/<date>-<slug>.md` with TL;DR, key claims, and a list of pages this source affects
4. Update affected `entities/` and `concepts/` pages (create new ones if needed)
5. Update `index.md` with any new pages
6. Append an entry to `log.md`
7. Report back: what was created, what was updated, any contradictions noticed

A single source typically touches **5–15** wiki pages. Ask the user before creating a new entity/concept page when a term appears in only one source — prefer inline mention until it shows up twice.

### Query

User asks a question against the wiki. Flow:

1. Read `index.md` first to find candidate pages
2. Read the candidate pages (follow `[[links]]` as needed)
3. Synthesize an answer with **page citations** (e.g. "per `concepts/rag.md`...")
4. Offer to file the answer as a new `syntheses/*.md` page if it has lasting value
5. If filed: update `index.md` and `log.md`

Never fabricate claims not supported by the wiki. If the wiki doesn't contain the answer, say so and suggest sources to ingest.

### Lint

User asks for a health check. Flow:

1. Find orphan pages (no inbound links) → propose links or deletion
2. Find dead links (pointing to nonexistent pages) → propose fixes
3. Find contradictions between pages → surface for user review
4. Find stale claims (newer sources contradict older ones) → propose revisions
5. Find concepts mentioned frequently but lacking their own page → propose creation
6. Append a lint summary to `log.md`

Do not auto-apply destructive changes (deletion, merging). Propose, then wait for user approval.

## Initialize a new wiki

When starting fresh, do this:

1. **Ask the user about domain and scope.** The schema should reflect the domain. Example questions:
   - "What's the primary use case? (reading a book / research topic / team knowledge / personal tracking / something else)"
   - "Do you have existing sources to seed with, or is this greenfield?"
   - "Where should the wiki live? (current directory, a subfolder, a new repo)"

2. **Create the default directory structure** (see above). Use `mkdir -p` for all folders.

3. **Copy `schema-template.md` to `<wiki-root>/AGENTS.md`** and customize it with:
   - The domain-specific focus (from step 1)
   - Any naming conventions the user prefers
   - Any frontmatter fields that matter for this domain

4. **Create starter files:**
   - `overview.md` — one paragraph explaining what this wiki is for
   - `index.md` — empty sections for each page type
   - `log.md` — a single `## [YYYY-MM-DD] init` entry

5. **If the user has seed sources**, ingest the first one together as a demo. This establishes the tone and lets the user correct the LLM's style early.

6. **Confirm the workflow** with the user: "From now on, drop sources in `raw/` and say 'ingest this'. I'll update the wiki."

## Key principles

- **The LLM writes; the human curates.** Do not ask the user to write pages. Do ask for direction on what to emphasize.
- **Every claim cites its source.** In `entities/` and `concepts/` pages, every non-obvious fact should link back to the `sources/*.md` page it came from.
- **Fewer, richer pages > many thin pages.** Create a new page only when the subject will plausibly accumulate content from multiple sources. Otherwise, mention it inline.
- **Links are mandatory, not decorative.** First occurrence of any entity/concept in any page must be `[[linked]]`. This is what makes the graph view valuable.
- **Syntheses are opinions; entities are facts.** Keep them in different folders. Syntheses can be challenged and revised; entities should change only when sources change.
- **The schema evolves.** When the user corrects the LLM's behavior, update `AGENTS.md` instead of just remembering for this session.
- **Never delete without confirmation.** Especially source pages and entity pages. Deletion is a user decision.

## Common pitfalls

- **Dumping raw source content into entity pages.** Entity pages should contain *digested* facts, not quotes. Quotes and deep detail belong in the `sources/` page.
- **Creating entity pages for every proper noun.** Most proper nouns only appear once and don't deserve their own page. Default to inline mention.
- **Forgetting to update `index.md`.** If it gets stale, queries break because the LLM can't find pages.
- **Mixing chronological and conceptual organization.** `log.md` is the only chronological file. Everything else is organized by meaning.
- **Letting `overview.md` rot.** Refresh it after every ~10 ingests or when the shape of the wiki changes.

## Progressive reference

- Full page templates with frontmatter: `page-templates.md`
- Detailed step-by-step workflows for ingest / query / lint: `workflows.md`
- Ready-to-drop `AGENTS.md` template for new wikis: `schema-template.md`
