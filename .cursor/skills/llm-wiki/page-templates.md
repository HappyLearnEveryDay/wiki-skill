# Page Templates

Use these as defaults when creating wiki pages. All pages use YAML frontmatter and Obsidian-style `[[wiki-links]]`.

## Source page

Path: `sources/<YYYY-MM-DD>-<slug>.md`

One written per ingested source. Written once, edited rarely.

```markdown
---
type: source
title: "<Original title>"
url: <original URL or omit>
author: <author or omit>
date_published: <YYYY-MM-DD or omit>
date_ingested: <YYYY-MM-DD>
medium: [article | paper | podcast | book-chapter | transcript | note]
tags: [<topical tags>]
---

# <Original title>

## TL;DR
Three sentences at most. What does this source argue or report?

## Key claims
- Claim 1 (with page/timestamp reference if applicable)
- Claim 2
- Claim 3

## Notable quotes
> Quote, with location if possible.

## Wiki impact
**Created:** [[entities/new-entity]], [[concepts/new-concept]]
**Updated:** [[entities/existing-entity]], [[concepts/existing-concept]]

## My take (optional)
Subjective judgment, skepticism, follow-up questions. Clearly marked as opinion.
```

## Entity page

Path: `entities/<slug>.md`

For named things: people, organizations, products, places, works, events.

```markdown
---
type: entity
aliases: [<other names this thing goes by>]
category: [person | organization | product | place | work | event | other]
tags: [<topical tags>]
first_seen: <YYYY-MM-DD>
sources:
  - [[sources/YYYY-MM-DD-slug]]
---

# <Canonical name>

> One-line gloss: what is this thing?

## Key facts
- Bullet list of objective, source-cited facts.
- Each significant fact should reference a source, e.g.: "Founded in 2015 ([[sources/2026-04-10-company-history]])"

## Relationships
- Connected to [[entities/other-entity]] because ...
- Instance of [[concepts/some-concept]]
- Compared with [[entities/rival]] in [[syntheses/rival-comparison]]

## Appears in
- [[sources/YYYY-MM-DD-slug-1]] — brief note on what this source says about it
- [[sources/YYYY-MM-DD-slug-2]] — brief note
```

## Concept page

Path: `concepts/<slug>.md`

For abstractions: methods, theories, patterns, techniques, frameworks.

```markdown
---
type: concept
aliases: [<alt names>]
tags: [<topical tags>]
first_seen: <YYYY-MM-DD>
sources:
  - [[sources/YYYY-MM-DD-slug]]
---

# <Concept name>

## Definition
One-paragraph definition in your (the user's) framing.

## Mechanism / how it works
Bullet list or short prose. Cite sources for non-obvious claims.

## Examples
- Example 1 — from [[sources/...]]
- Example 2

## Strengths / limitations
- Strength: ... ([[sources/...]])
- Limitation: ... ([[sources/...]])

## Related concepts
- Contrasts with [[concepts/other-concept]] — see [[syntheses/comparison]]
- Builds on [[concepts/prerequisite]]
- Subsumed by [[concepts/broader]]

## Appears in
- [[sources/...]] — what this source contributes
```

## Synthesis page

Path: `syntheses/<slug>.md`

Written when the user asks a comparative / analytical question. Represents the LLM's (and user's) interpretation, not a plain source summary.

```markdown
---
type: synthesis
tags: [<topical tags>]
date: <YYYY-MM-DD>
triggered_by: "<the user question or context that prompted this>"
draws_on:
  - [[sources/...]]
  - [[concepts/...]]
  - [[entities/...]]
---

# <Title — usually the question or comparison>

## Question
What was being asked.

## Short answer
A paragraph or a single table.

## Analysis
Detailed reasoning, with inline citations to the pages it draws on.

## Open questions
Things this analysis surfaced but didn't resolve.

## Status
draft | reviewed | superseded-by-[[syntheses/newer]]
```

## Question page (optional)

Path: `questions/<slug>.md`

For open questions and hypotheses that warrant tracking.

```markdown
---
type: question
tags: [<topical tags>]
opened: <YYYY-MM-DD>
status: [open | partial | answered | abandoned]
---

# <Question as a sentence>

## Why it matters
Briefly, why is this worth tracking?

## What we know
- Evidence for — [[sources/...]]
- Evidence against — [[sources/...]]

## Missing
- What sources would help resolve this?
- What experiments / follow-up would?

## Answer (fill when resolved)
If answered, summarize and link to the definitive [[syntheses/...]] or [[concepts/...]] page.
```

## Index page

Path: `index.md`

```markdown
# Index

Last updated: <YYYY-MM-DD>

## Overview
- [[overview]]

## Entities
- [[entities/alpha]] — one-line summary
- [[entities/beta]] — one-line summary

## Concepts
- [[concepts/foo]] — one-line summary
- [[concepts/bar]] — one-line summary

## Syntheses
- [[syntheses/rag-vs-wiki]] — comparison of retrieval vs compilation approaches
- [[syntheses/...]] — ...

## Questions (open)
- [[questions/...]] — ...

## Sources (newest first)
- <YYYY-MM-DD> [[sources/YYYY-MM-DD-slug]] — Title
- ...
```

Keep each line to a link + short gloss. The index is for the LLM to navigate; don't bloat it with full descriptions.

## Log page

Path: `log.md`

Append-only chronological record. Every entry starts with a consistent header prefix for easy grep.

```markdown
# Log

## [YYYY-MM-DD] init | wiki created
Initialized structure for <domain>.

## [YYYY-MM-DD] ingest | <source title>
Created [[sources/YYYY-MM-DD-slug]]. Touched N pages: [[entities/...]], [[concepts/...]].
Notes: <any contradictions or surprises>.

## [YYYY-MM-DD] query | <question>
Filed as [[syntheses/...]].

## [YYYY-MM-DD] lint
Found 2 orphans, 1 dead link. User approved fixes.
```

The `## [YYYY-MM-DD] <action> | <subject>` prefix is important — it allows `grep "^## \[" log.md` to produce a clean timeline.

## Overview page

Path: `overview.md`

```markdown
---
type: overview
updated: <YYYY-MM-DD>
---

# <Wiki title> — Overview

## What this wiki is about
One paragraph: the domain, the purpose, the intended audience (usually just you).

## Current state
- <N> sources ingested
- <N> entities tracked
- <N> concepts mapped
- <N> open questions

## Main threads
- **Thread A**: short description, starting at [[concepts/...]]
- **Thread B**: short description, starting at [[concepts/...]]

## Recent focus
What has been the center of attention in the last few ingests.

## How to use
Short note to future-you or collaborators: how to read this wiki, what the conventions are, where to start.
```
