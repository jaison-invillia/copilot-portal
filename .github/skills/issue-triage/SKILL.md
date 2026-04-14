---
name: issue-triage
description: "Complete issue triage workflow: from raw demand to implementation-ready issue. Orchestrates Product Owner → Architect → DBA(if DB) → Staff planning. Use when a new demand arrives and needs full triage through the agent pipeline."
argument-hint: "Describe the demand or provide the issue number"
---

# Issue Triage Skill

End-to-end workflow for triaging a new demand through the agent pipeline until it is ready for implementation.

## When to Use

- A new business demand or feature request arrives
- An existing issue needs full triage (PO refinement + architectural analysis + implementation planning)
- You want to run the complete pipeline: PO → Architect → Staff planning

## Procedure

### Phase 1 — Product Owner Refinement

1. Invoke the `product-owner` agent with the demand
2. The PO will:
   - Clarify ambiguities (may ask questions)
   - Search for existing cards/issues/tickets when tracker access is available
   - Produce a structured draft with acceptance criteria, priority, and subtask checklist
   - Wait for explicit approval before any tracker write
3. If approved and writable MCP support exists, wait for the card to be created/updated
4. If no writable MCP support exists, stop triage in `draft-only` mode and hand off the structured draft for manual creation

### Phase 2 — Architectural Analysis

1. Invoke the `architect` agent with the approved card/issue reference from Phase 1
2. The Architect will:
   - Fetch the card/issue via the configured tracker when available
   - Read all mandatory documentation
   - Analyze affected layers and file structure
   - Check if an ADR is needed
   - Post the architectural plan as a comment on the approved card when the tracker supports comments
3. Wait for the analysis to be posted

### Phase 2.5 — Database Analysis (conditional)

1. If the architectural analysis indicates database impact, invoke the `dba` agent with the approved card/issue reference
2. The DBA will:
   - Analyze schema/migration/constraint/index impact
   - Flag rollout and rollback risks
   - Post database recommendations on the approved card when tracker supports comments
3. Wait for the analysis to be posted

### Phase 3 — Implementation Planning

1. Invoke the `staff` agent with the approved card/issue number
2. The Staff will:
   - Clarify ambiguities and validate task quality
   - Trigger `documenter` for mandatory mini documentation plan
   - Consult `dba` before backend delegation when DB changes are in scope
   - Read the PO's task and Architect's analysis
   - Plan implementation at code level
   - Document the plan on the issue
   - Classify testing approach as `feature_nova` or `mudanca_existente`
   - Consult test-advisor for testing strategy by classification
3. The approved card is now **implementation-ready**

## Triage Criteria

Refer to [triage criteria](./references/triage-criteria.md) for priority definitions and completeness checks.

## Completion Checklist

- [ ] Issue exists with clear title and description
- [ ] Card draft was shown and explicitly approved before creation/update
- [ ] Acceptance criteria defined (Given/When/Then)
- [ ] Priority assigned (P0–P3)
- [ ] Architectural analysis posted as comment
- [ ] DBA analysis posted when DB impact exists
- [ ] ADR drafted if needed
- [ ] Implementation plan documented
- [ ] Documentation mini-plan outlined
- [ ] Testing strategy outlined by classification
- [ ] Subtask checklist in card body
