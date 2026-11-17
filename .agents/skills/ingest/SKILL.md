---
name: ingest
description: Ingest and process a raw document from raw/ into the Obsidian knowledge base wiki. Activates when the user asks to ingest, process, or extract notes from a source file, or uses the /ingest command.
---

# Ingest Workflow Skill

This skill guides the agent through processing a raw source file from `raw/` into the knowledge base wiki, adhering strictly to [SCHEMA.md](../../../SCHEMA.md).

## Rules & Constraints

1. **One source at a time**: Never batch-ingest multiple sources in one turn unless explicitly instructed.
2. **`raw/` is READ ONLY**: Never modify, rename, or delete any file in `raw/` or `raw/assets/`.
3. **Mandatory user checkpoint**: Always discuss 3–5 key takeaways with the user and **wait for their reply** before creating or updating any wiki pages.
4. **Wikilinks syntax**: Use Obsidian wikilinks `[[page-name]]` or `[[page-name|display text]]`. Never use Markdown file links like `[text](path.md)`.
5. **No invented citations**: Every claim must cite its source page inline: `... (see [[source-page-name]])`.
6. **No silent overwrites**: If a claim contradicts existing wiki content, keep both claims, set `status: contested`, and cite both sources.

---

## Step-by-Step Procedure

### Step 1: Read & Analyze Source
- Read the specified source file in `raw/<filename>.md` completely.
- Extract:
  - Original title, author, date, and core subject matter.
  - 3–5 key takeaways (the most impactful concepts, architecture decisions, or insights).
  - Potential concepts, entities, or problems to create or update.

### Step 2: Discuss Takeaways (Checkpoint)
- Present the 3–5 key takeaways to the user clearly.
- List the proposed wiki pages to create or update.
- Ask the user what to emphasize or if any adjustments are needed.
- **STOP and wait for the user's confirmation or guidance.**

### Step 3: Create Source Page (`wiki/sources/`)
After user confirmation, create `wiki/sources/<source-name>.md` with YAML frontmatter:

```yaml
---
type: source
title: Human Readable Source Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
tags: [tag1, tag2]
status: stable
---
```

Include:
- Metadata: Original title, author, date, path in `raw/`
- Key Takeaways (bullet points)
- Notable Quotes (short, with context)
- Pages created or updated by this source

### Step 4: Create or Update Concept, Entity, Problem Pages
For each touched concept/entity/problem:
- Use lowercase kebab-case for filenames.
- Adhere to the standard page template:
  - Frontmatter (`type`, `title`, `created`, `updated`, `sources`, `tags`, `status`)
  - 1–3 sentence introductory summary
  - Core content citing the source: `(see [[source-page-name]])`
  - `## Related` section linking to relevant pages
- Keep pages focused (split if exceeding ~800 words).
- Verify contradictions: If conflicting information is detected, do not overwrite; mark `status: contested` and link both sources.

### Step 5: Update Index
- Add newly created pages to their appropriate categories in `wiki/index.md` (`## Sources`, `## Concepts`, `## Entities`, `## Problems`, `## Topics`).

### Step 6: Log Activity
- Append an entry to `wiki/log.md`:
  ```markdown
  ## [YYYY-MM-DD] ingest | <Source Title>
  - Source: raw/<file>
  - Created: [[page-1]], [[page-2]]
  - Updated: [[page-3]]
  - Contradictions: none | [[x]] vs [[y]]
  ```

### Step 7: Final Report
- Report back to the user:
  - Source page created.
  - List of concept/entity pages created or updated.
  - Contradictions identified (if any).
  - Confirmation of `wiki/index.md` and `wiki/log.md` updates.
