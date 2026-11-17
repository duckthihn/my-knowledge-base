# Vault Prompts & Slash Commands

You can use the built-in custom skills via slash commands, or use the prompt templates below:

### Ingest
- **Slash command**: `/ingest raw/<filename>.md`
- **Manual prompt**: `Read SCHEMA.md and run the INGEST workflow for raw/<filename>.md. Discuss key takeaways with me first.`

### Query
- **Slash command**: `/query <question or topic>`
- **Manual prompt**: `Read SCHEMA.md, then follow the QUERY workflow: <question>`

### Lint
- **Slash command**: `/lint`
- **Manual prompt**: `Read SCHEMA.md and run the LINT workflow. Report issues before fixing anything.`
