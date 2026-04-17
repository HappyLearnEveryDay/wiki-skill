# Workflows

Detailed step-by-step procedures for the three core operations. Follow these in order; don't skip steps unless the user explicitly redirects.

## Ingest workflow

Triggered when the user says "ingest this", "add this to the wiki", drops a new file in `raw/`, or shares a URL.

### Checklist

```
- [ ] 1. Locate and read the source
- [ ] 2. Discuss takeaways briefly with user
- [ ] 3. Identify affected pages
- [ ] 4. Write source summary
- [ ] 5. Update/create entity & concept pages
- [ ] 6. Update index.md
- [ ] 7. Append log.md entry
- [ ] 8. Report back with summary
```

### Step 1: Locate and read the source

- If the source is a URL, ask how to fetch it (or whether the user has already clipped it to `raw/`).
- If the source is a file path, read the full content.
- For images referenced in markdown, read the text first. View images only if the text references them meaningfully.

### Step 2: Discuss takeaways

Before writing anything, share a brief (3–5 bullet) readout with the user:
- What this source is
- Its core claim(s)
- What stood out as novel vs. reinforcing existing wiki content
- Any concerns about credibility or bias

Let the user steer emphasis. Do not proceed to write pages until the user signals to continue (or says something like "go ahead").

### Step 3: Identify affected pages

Before editing, list (in your reply or internally):
- **New entities** this source introduces — but apply the "two-source rule": unless the entity seems core to the user's domain, prefer deferring page creation until it appears again.
- **New concepts** this source introduces — same two-source rule.
- **Existing pages to update** — grep or read `index.md` to find them.
- **Potential contradictions** — where does this source disagree with existing pages?

Share this list with the user if it's non-trivial. Otherwise, proceed.

### Step 4: Write source summary

Create `sources/<YYYY-MM-DD>-<slug>.md` using the source template (see `page-templates.md`). The slug should be short, kebab-case, and derived from the title.

### Step 5: Update/create entity & concept pages

For each affected page:
- **Creating**: use the appropriate template. Start lean — only write what this source supports.
- **Updating**: add new facts under existing sections, citing the new source. Add to "Appears in" list.
- **Handling contradictions**: do not overwrite. Add a note like "However, [[sources/new]] reports X, contradicting the earlier claim from [[sources/old]]." Surface this to the user.

### Step 6: Update `index.md`

Add any newly created pages to the appropriate sections with one-line glosses. Update the "Last updated" date.

### Step 7: Append `log.md` entry

Use the format:

```markdown
## [YYYY-MM-DD] ingest | <source title>
Created [[sources/YYYY-MM-DD-slug]]. Touched <N> pages: [[a]], [[b]], [[c]].
Notes: <anything noteworthy — contradictions, gaps discovered, questions raised>
```

### Step 8: Report back

Tell the user:
- What was created (paths)
- What was updated (paths with one-line notes per page)
- Any contradictions or open questions surfaced
- Suggested next source or question to explore (if any)

Don't quote full page contents in the report — the user can read the files. Keep the report under ~10 lines.

---

## Query workflow

Triggered when the user asks a question that seems to be about the wiki's domain.

### Checklist

```
- [ ] 1. Read index.md
- [ ] 2. Identify candidate pages
- [ ] 3. Read candidates; follow links as needed
- [ ] 4. Synthesize answer with citations
- [ ] 5. Offer to file as synthesis
- [ ] 6. If filed: update index.md + log.md
```

### Step 1: Read `index.md`

Always start here. It's faster than scanning the filesystem and it reflects the curated view of what exists.

### Step 2: Identify candidates

Pick 3–7 pages that seem most likely to contain the answer. If the index isn't specific enough, grep page titles or frontmatter for keywords.

### Step 3: Read candidates

Read each fully. Follow `[[links]]` when a page defers to another (e.g., "see [[syntheses/x]] for details"). Don't bottomlessly recurse — 2 levels deep is usually enough.

### Step 4: Synthesize

Produce an answer that:
- **Cites pages inline** — e.g., "Per `entities/obsidian.md`, ..."
- **Never fabricates claims.** If the wiki is silent on something, say so: "The wiki doesn't contain data on X. Consider ingesting a source on this."
- **Surfaces contradictions** you notice while reading.

### Step 5: Offer to file as synthesis

If the answer has lasting value (non-trivial, reusable, or synthesizes multiple pages), say:

> "Want me to file this as `syntheses/<slug>.md`?"

Trivial answers (one-fact lookups) do not need filing.

### Step 6: If filed

- Write `syntheses/<slug>.md` using the synthesis template.
- Update `index.md` with the new entry.
- Append to `log.md`:
  ```
  ## [YYYY-MM-DD] query | <question>
  Filed as [[syntheses/slug]]. Drew on: [[a]], [[b]], [[c]].
  ```

---

## Lint workflow

Triggered when the user asks for a "health check", "lint", "cleanup", or periodically (user's choice, default every 10–20 ingests).

### Checklist

```
- [ ] 1. Orphan check
- [ ] 2. Dead link check
- [ ] 3. Contradiction check
- [ ] 4. Stale claim check
- [ ] 5. Missing page check
- [ ] 6. Index / overview freshness
- [ ] 7. Propose actions; wait for approval
- [ ] 8. Apply approved actions
- [ ] 9. Append log.md entry
```

### Step 1: Orphan check

Find pages with no inbound links.
- For each orphan: is it genuinely isolated, or is it central but under-linked?
- Propose either: add links from related pages, or delete.

### Step 2: Dead link check

Find `[[links]]` pointing to files that don't exist.
- Is the target a typo? Propose a rename.
- Is it a page that should exist but doesn't? Propose creating a stub, or removing the link.

### Step 3: Contradiction check

Scan pages (especially entities/ and concepts/) for claims that contradict each other. Look at source dates — is newer information superseding older?
- Never auto-resolve. Surface contradictions to the user with page references.

### Step 4: Stale claim check

Find claims in pages that are older than the most recent source touching that topic. Propose revisions, citing the newer source.

### Step 5: Missing page check

Look for terms that appear `[[linked]]` or frequently mentioned across ≥2 pages but lack their own page. Propose creation.

### Step 6: Index / overview freshness

- Is `index.md` up to date with all files?
- When was `overview.md` last updated? If more than ~10 ingests ago, propose refresh.

### Step 7: Propose actions

Produce a single report:

```markdown
## Lint report — YYYY-MM-DD

### Orphans (N)
- [[path]] — suggest: link from [[other]] / delete

### Dead links (N)
- [[a]] → [[missing]] — suggest: ...

### Contradictions (N)
- [[a]] says X, but [[b]] says ¬X — needs user review

### Stale claims (N)
- [[a]] claim outdated by [[sources/newer]] — suggest revision

### Missing pages (N)
- "Foo" referenced in [[a]], [[b]] — suggest creating [[concepts/foo]]

### Housekeeping
- index.md missing entries for: ...
- overview.md last updated YYYY-MM-DD (N ingests ago) — suggest refresh
```

### Step 8: Apply approved actions

Wait for the user to approve each category (or all). Apply the approved ones. Never delete or merge pages without explicit user sign-off.

### Step 9: Append log.md entry

```markdown
## [YYYY-MM-DD] lint
<summary of findings and what was applied>
```

---

## Schema evolution

Not a routine operation, but important.

When the user corrects the LLM's behavior during a session — e.g., "don't create entity pages for one-off mentions" or "I want tags on every page" — do two things:

1. Apply the correction in the current session.
2. **Update `AGENTS.md`** in the wiki root so the rule sticks for future sessions.

Always confirm the `AGENTS.md` change with the user: quote the old text and the new text, and ask "apply this?"

Do not edit this skill's files (in `.cursor/skills/llm-wiki/`) based on per-wiki corrections. Those are wiki-specific and belong in the wiki's own `AGENTS.md`.
