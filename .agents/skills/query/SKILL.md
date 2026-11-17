---
name: query
description: Answer questions, synthesize insights, and produce structured guides from the Obsidian knowledge base wiki. Activates when the user asks a question about topics in the wiki or uses the /query command.
---

# Query Workflow Skill

This skill guides the agent in answering user queries using solely the knowledge base vault and formatting any output documents according to [SCHEMA.md](../../../SCHEMA.md).

## Rules & Constraints

1. **Vault as Single Source of Truth**: Always read `wiki/index.md` first to locate relevant pages, then read those pages.
2. **Strict Inline Citations**: Answer from the wiki, citing pages with `[[wikilinks]]`.
3. **Transparent Knowledge Boundaries**: Explicitly state what the wiki does **not** cover. Do not fill gaps from memory without labeling it as outside knowledge.
4. **Standardized Output Format**: When saving synthesis to `outputs/<topic>.md`, follow the exact schema template.
5. **Log on Save**: Only append to `wiki/log.md` if an output file was saved or wiki pages were updated.

---

## Step-by-Step Procedure

### Step 1: Discover Relevant Wiki Pages
- Read `wiki/index.md` to identify candidate pages covering the question.
- Read each relevant wiki page in full to gather factual context.

### Step 2: Formulate Answer
- Compose a clear, authoritative response based on the wiki pages.
- Cite specific wiki concepts with `[[wikilinks]]` inline (e.g. `(xem [[page-name]])`).
- Include a clear statement of gaps or topics not covered in the wiki.
- If outside technical context is added to help the user, explicitly label it: *(Lưu ý: phần này dựa trên kiến thức mở rộng ngoài wiki)*.

### Step 3: Offer to Save Output
- If the synthesis is comprehensive, valuable, or if the user requests saving it:
  - Propose saving to `outputs/<topic-kebab-case>.md`.
  - If it contains new architectural synthesis, suggest folding key points back into `wiki/topics/`.

### Step 4: Write Output Document (If Approved)
Save to `outputs/<topic>.md` using this exact standardized format:

```markdown
# <Topic Title in English or Vietnamese>

**Date**: YYYY-MM-DD  
**Sources**: [[page-1]], [[page-2]], ...

---

<1-2 sentence lead-in / summary>

## 1. <Section Title>
...

## 2. <Section Title>
...

## N. Phạm vi wiki chưa đề cập
- <Specific points or gaps not currently covered by vault sources>

## Sources Consulted

- [[page-1]]
- [[page-2]]
```

Formatting rules:
- **Heading 1**: Clean, descriptive topic title (never prefix with `# Query:`).
- **Metadata**: `**Date**` and `**Sources**` directly below title before the first `---`.
- **Headings**: Systematic numbering (`## 1.`, `## 2.`, etc.).
- **Boundary**: Always keep `## N. Phạm vi wiki chưa đề cập`.
- **Footer**: `## Sources Consulted` list at the bottom.

### Step 5: Log Activity
If an output file was saved or wiki pages updated, append to `wiki/log.md`:

```markdown
## [YYYY-MM-DD] query | <Query Subject>
- Sources consulted: [[page-1]], [[page-2]]
- Saved output: outputs/<filename>.md
- Created: [[new-page]] (or none)
- Updated: [[page]] (or none)
- Contradictions: none
```
