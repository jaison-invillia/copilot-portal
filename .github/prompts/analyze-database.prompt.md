---
description: "Trigger the DBA agent to perform a database impact analysis for an existing GitHub Issue."
agent: "dba"
argument-hint: "Issue number (e.g., #42 or 42)"
---

Perform a complete database analysis for the specified GitHub Issue:

1. Fetch the issue via MCP and read all context/comments
2. Read mandatory documentation (`docs/database.md`, `docs/domain.md`, `docs/architecture.md`, related ADRs)
3. Identify schema impact (tables/collections, fields, keys, constraints)
4. Define migration strategy with rollback safety (`up`/`down`)
5. Recommend indexes and performance safeguards
6. Highlight integrity/security risks and mitigation
7. Indicate documentation impact and request `documenter` when needed
8. Post the structured DBA analysis as a comment on the issue via MCP
9. Update issue status with your progress

Follow the DBA agent workflow strictly.
