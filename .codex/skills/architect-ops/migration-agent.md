---
name: migration-agent
description: Executes safe, planned multi-file refactors. OpenAI Codex compatible.
---

# Architect-Ops: Migration Agent (Codex)

See `.claude/skills/architect-ops/migration-agent.md` for the full specification.

## Codex Usage

```
"Migrate all direct fetch() calls to use ApiClient across the codebase"
"Replace moment.js with date-fns in every file"
"Consolidate the duplicate formatCurrency functions into utils/format.ts"
```

**Note:** Migration Agent always presents a plan and waits for your explicit
"execute" confirmation before applying any changes.
