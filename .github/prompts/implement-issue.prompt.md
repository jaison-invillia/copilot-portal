---
description: "Trigger the Staff orchestrator to plan and execute the implementation of a GitHub Issue, delegating to backend/frontend sub-agents."
agent: "staff"
argument-hint: "Issue number (e.g., #42 or 42)"
---

Plan and execute the implementation for the specified GitHub Issue:

1. Fetch the issue and read all context (PO task, Architect analysis)
2. Read all mandatory project documentation
3. Clarify ambiguities and validate task quality before implementation
4. Trigger documenter for mandatory mini documentation plan at task start
5. If DB impact exists, consult `dba` before backend delegation
6. Classify the task as `feature_nova` or `mudanca_existente`
7. Plan implementation at code level (files to create/modify, order)
8. Document the plan on the issue as a comment via MCP
9. Consult test-advisor using the classification
10. Delegate to backend-dev and/or frontend-dev in parallel when possible
11. Validate results and run tests
12. Consult metrifier for observability recommendations
13. Trigger reviewer only if code changes exist
14. Create a feature branch and open a PR via MCP
15. Post final status update on the issue

Follow the staff agent workflow strictly.
