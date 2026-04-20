# AGENTS.md template for a new wiki

This is a ready-to-drop `AGENTS.md` template. Copy it to `<wiki-root>/AGENTS.md` during initialization and customize the sections marked `<!-- customize -->`.

This file is the **source of truth** for how the wiki is maintained. When the user corrects conventions, update this file — not the skill.

---

```markdown
# Wiki Conventions

<!-- customize: one paragraph describing what this wiki is about and who uses it -->
This wiki tracks [domain]. It is maintained by an LLM agent using the llm-wiki skill.
The human curates sources and asks questions; the LLM writes and links all pages.

## Layout

- `raw/` — original sources. Immutable. Never edit.
- `sources/` — one summary per ingested source. Written once.
- `entities/` — named things (people, orgs, products, works). Accumulates facts.
- `concepts/` — abstractions (methods, patterns, theories). Accumulates examples.
- `syntheses/` — comparisons and analyses written in response to queries.
- `questions/` — open questions and hypotheses. <!-- customize: remove if unused -->
- `index.md` — content catalog.
- `log.md` — chronological record.
- `overview.md` — top-level synthesis.

## Naming

- All paths are lowercase kebab-case: `entities/vannevar-bush.md`
- Source files are prefixed with ingest date: `sources/2026-04-15-llm-wiki-article.md`
- Slugs should be short and unambiguous. When disambiguating, prefer suffixes: `entities/apple-company.md` vs `entities/apple-fruit.md`.

## Frontmatter

Every page uses YAML frontmatter. Required fields by type:

- **source**: `type`, `title`, `date_ingested`, `medium`, `tags`
- **entity**: `type`, `category`, `tags`, `first_seen`, `sources`
- **concept**: `type`, `tags`, `first_seen`, `sources`
- **synthesis**: `type`, `tags`, `date`, `draws_on`
- **question**: `type`, `tags`, `opened`, `status`

<!-- customize: add domain-specific fields here, e.g. `rating`, `difficulty`, `status` -->

## Linking

- Use `[[relative/path/without-extension]]` for all internal links.
- The **first** occurrence of any entity or concept on any page must be linked.
- Subsequent mentions on the same page can be plain text.
- Every non-obvious claim in `entities/` and `concepts/` pages cites a `[[sources/...]]` page.

## Page discipline

- **Entities**: objective facts only. Opinions go in syntheses.
- **Concepts**: definitions, mechanisms, examples. Keep prose tight.
- **Sources**: do not copy the full source — distill to TL;DR + key claims.
- **Syntheses**: clearly marked as interpretation; always list what they draw on.
- **New pages**: only create an entity or concept page when the subject appears in ≥2 sources (or the user explicitly requests).

## Workflows

Ingest, query, and lint workflows follow the defaults in the `llm-wiki` skill.
See the skill's `workflows.md` for the full checklists.

<!-- customize: note any overrides to the default workflows -->

## Log format

Every log entry starts with `## [YYYY-MM-DD] <action> | <subject>` so that
`grep "^## \[" log.md` produces a clean timeline.

Actions: `init`, `ingest`, `query`, `lint`, `refactor`, `schema-change`.

## Linter rules

Run the lint workflow every ~10 ingests or on user request. Check for:
- Orphan pages (no inbound links)
- Dead links (target doesn't exist)
- Contradictions between pages
- Stale claims superseded by newer sources
- Terms mentioned across pages but lacking their own page

Never delete or merge pages without explicit user approval.

## Boundaries — what the LLM can do without asking

Allowed:
- Create new entity/concept/synthesis pages (when the two-source rule is met, or user requested)
- Add facts to existing pages, with citations
- Update `index.md` and `log.md`
- Fix typos in links and filenames

Requires user approval:
- Deleting any page
- Merging two pages
- Renaming an existing page
- Changing conventions in this file
- Marking a claim as contradicted or superseded

<!-- customize: adjust these if you want more/less autonomy -->

## Evolution

When the user corrects a convention during a session, update this file in the same session. Quote the old text, show the new text, confirm, then apply.
```
