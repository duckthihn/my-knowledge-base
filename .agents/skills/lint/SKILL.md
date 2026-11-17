---
name: lint
description: Perform a health check and audit of the Obsidian knowledge base vault (broken links, orphan pages, missing citations, index drift). Activates when the user asks to lint, audit, or clean the wiki, or uses the /lint command.
---

# Lint Workflow Skill

This skill guides the agent in running a comprehensive audit of the knowledge base vault and fixing identified issues in accordance with [SCHEMA.md](../../../SCHEMA.md).

## Rules & Constraints

1. **Check and report first**: Always report findings to the user and **wait for agreement** before modifying any files.
2. **Never delete without permission**: Prefer adding links or merging pages with redirect notes rather than deleting.
3. **Keep edits small and reviewable**: The vault is under Git version control.
4. **Never touch `raw/` or `.obsidian/`**.

---

## Step-by-Step Procedure

### Step 1: Execute Vault Health Checks
Scan across all markdown files in `wiki/` and `raw/` to check:

1. **Broken Wikilinks**: Links `[[target]]` where `target.md` does not exist in `wiki/`.
2. **Orphan Pages**: Pages in `wiki/` with zero inbound wikilinks from other wiki pages.
3. **Missing Frontmatter or Citations**: Pages lacking standard YAML metadata (`type`, `title`, `created`, `updated`, `sources`, `tags`, `status`) or missing source references.
4. **Contested Pages**: Pages marked `status: contested` that may now be resolvable.
5. **Duplicate / Overlapping Pages**: Pages covering identical concepts that should be consolidated.
6. **Concept Candidates**: Key terms mentioned frequently across multiple pages that do not yet have a dedicated concept/entity page.
7. **Index Drift**: Pages listed in `wiki/index.md` that no longer exist, or existing pages missing from `wiki/index.md`.
8. **Page Length**: Focused pages exceeding ~800 words that should be split.
9. **Un-ingested Raw Files**: Markdown files in `raw/` that do not have a corresponding source page in `wiki/sources/`.

### Step 2: Present Audit Report (Checkpoint)
- Present findings clearly structured by category (e.g., using Markdown tables or checklists).
- For each issue, propose the specific remediation action (e.g., "Add inbound link from `[[parent-page]]` to resolve orphan").
- **STOP and wait for the user to approve the remediation plan.**

### Step 3: Apply Fixes (After Approval)
- Link orphan pages from relevant parent concept or topic pages.
- Correct broken wikilinks or create missing stubs if necessary.
- Synchronize `wiki/index.md` with the filesystem.
- Normalize formatting (ensure valid YAML frontmatter and wikilinks).

### Step 4: Log Activity
Append an entry to `wiki/log.md`:

```markdown
## [YYYY-MM-DD] lint | Full vault lint check
- Issues found: <summary of issues>
- Fixed: <summary of changes applied>
- Contradictions: none
```

### Step 5: Final Report
Confirm to the user that all approved fixes have been applied and logged.
