# 🧠 Second Brain

A personal knowledge base built as an **Obsidian vault** and maintained by an AI agent. You drop raw source material in; the agent reads, synthesizes, and writes structured wiki pages, keeping everything interlinked, cited, and consistent.

## How it works

```
You add a source → AI reads & synthesizes → Wiki pages written & linked
You ask a question → AI queries the wiki → Cited answer, optionally saved
```

The agent follows three workflows defined in [`SCHEMA.md`](SCHEMA.md):

| Workflow | Trigger | What happens |
|---|---|---|
| **INGEST** | Drop a file into `raw/`, run the Ingest prompt | Agent reads the source, discusses key takeaways with you, then creates/updates wiki pages |
| **QUERY** | Run the Query prompt with your question | Agent searches `wiki/index.md`, reads relevant pages, answers with citations, can save to `outputs/` |
| **LINT** | Run the Lint prompt | Agent checks for broken links, orphan pages, missing frontmatter, duplicates — reports before fixing |

## Folder structure

```
my-knowledge-base/
│
├── raw/                  # Source material — READ ONLY. Drop files here.
│   └── assets/           # Images and attachments for raw sources.
│
├── wiki/                 # Everything the AI writes and maintains.
│   ├── index.md          # Catalog of every wiki page. Updated on every ingest.
│   ├── log.md            # Append-only activity log (ingests, queries, lints).
│   ├── sources/          # One page per ingested source (takeaways, quotes, links).
│   ├── concepts/         # Explanations of ideas, mechanisms, patterns.
│   ├── entities/         # People, orgs, tools, projects.
│   ├── problems/         # Specific problems and their solutions / tradeoffs.
│   └── topics/           # Cross-domain synthesis pages.
│
├── outputs/              # Saved query answers and reports.
│
├── SCHEMA.md             # The agent's operating rules (source of truth).
├── PROMPTS.md            # Copy-paste prompts for Ingest / Query / Lint.
└── README.md             # This file.
```

## Quickstart

### 1 — Add a source

Copy a note, article, transcript, or doc into `raw/`. Then prompt the agent:

```
Read SCHEMA.md. Process raw/<filename>.md.
Discuss key takeaways with me first, then follow the INGEST workflow.
```

The agent will discuss 3–5 takeaways, wait for your direction, then write the wiki pages.

### 2 — Ask a question

```
Read SCHEMA.md, then follow the QUERY workflow: <your question here>
```

The agent answers from the wiki only, clearly marking any gaps. You can then ask it to save the answer to `outputs/`.

### 3 — Run a lint check (suggested monthly)

```
Read SCHEMA.md and run the LINT workflow. Report issues before fixing anything.
```

The agent will find broken links, orphan pages, missing citations, and duplicate content — and ask before touching anything.

## Design principles

- **Cite everything.** Every factual claim in the wiki links back to a source page, which links back to a file in `raw/`.
- **Never invent.** The agent won't fabricate sources, quotes, or links. If the wiki doesn't cover something, it says so.
- **Immutable sources.** Files in `raw/` are never modified or deleted by the agent.
- **Conflict-aware.** If a new source contradicts an existing wiki page, the agent flags it as `status: contested` instead of silently overwriting.
- **Git-friendly.** All changes are small and reviewable. Every edit is diffable.

## Page anatomy

Every wiki page carries YAML frontmatter:

```yaml
---
type: source | concept | entity | problem | topic
title: Human readable title
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["[[source-page-name]]"]
tags: [tag1, tag2]
status: draft | stable | contested
---
```

Internal links use Obsidian wikilink syntax: `[[page-name]]` or `[[page-name|display text]]`.

## Stack

- **[Obsidian](https://obsidian.md)** — local-first markdown editor with graph view and wikilinks.
- **[Antigravity (AGY)](https://antigravity.google/product/antigravity-cli/)** — AI agent that reads `SCHEMA.md` and runs the workflows.
- **Git** — version control for all wiki edits; every agent change is diffable.
