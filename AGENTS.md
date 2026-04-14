# AGENTS.md

This document defines the **AI agent squad** used in this repository, their responsibilities, delegation model, and collaboration flow.

It is intended for use with **GitHub Copilot, AI coding agents, and automated development workflows**.

---

## Core Principle

Documentation under `/docs` is the **source of truth**.

Agents must always consult documentation before generating code.

If a required change is not reflected in the documentation, the agent must:
1. Propose a documentation update
2. Only then generate code

---

## Mandatory Documentation

Agents must read the following files before implementing changes:

- `README.md`
- `docs/ai/ai-context.md`
- `docs/architecture.md`
- `docs/domain.md`
- `docs/database.md`
- `docs/api-spec.md`
- `docs/engineer-guidelines.md`
- `docs/security.md`
- `docs/observability.md`
- `docs/project-structure.md`
- `docs/agent-task-flow.md`
- `docs/adr/*`

---

## Agent Squad Overview

| # | Agent | File | Invocable? | Tools | Delegates to |
|---|-------|------|------------|-------|-------------|
| 1 | **Product Owner** | `product-owner.agent.md` | ✅ Yes | read, search, github/* | — |
| 2 | **Architect** | `architect.agent.md` | ✅ Yes | read, search, github/* | — |
| 3 | **Staff (Orchestrator)** | `staff.agent.md` | ✅ Yes | read, edit, search, execute, github/*, agent | backend-dev, frontend-dev, test-advisor, qa, metrifier, reviewer, documenter |
| 4 | **DBA** | `dba.agent.md` | ✅ Yes | read, search, github/* | documenter |
| 5 | **Backend Developer** | `backend-dev.agent.md` | ❌ Sub-agent | read, edit, search, execute | test-advisor |
| 6 | **Frontend Developer** | `frontend-dev.agent.md` | ❌ Sub-agent | read, edit, search, execute | test-advisor |
| 7 | **Test Advisor** | `test-advisor.agent.md` | ✅ Yes | read, search | — |
| 8 | **QA** | `qa.agent.md` | ✅ Yes | read, search, execute | — |
| 9 | **Reviewer** | `reviewer.agent.md` | ✅ Yes | read, search, github/* | — |
| 10 | **Documenter** | `documenter.agent.md` | ✅ Yes | read, edit, search, github/* | — |
| 11 | **Metrifier** | `metrifier.agent.md` | ✅ Yes | read, search | — |
| 12 | **Project Setup** | `project-setup.agent.md` | ✅ Yes | read, edit, search, execute, github/* | — |
| 13 | **Pathfinder** | `pathfinder.agent.md` | ✅ Yes | read, search | — |

---

## Agent Responsibilities

### 1. Product Owner (`product-owner`)

Converts business demands into clear, actionable tracker cards/issues/tickets.

**Responsibilities:**
- Clarify ambiguous demands (blocking gate)
- Define acceptance criteria (Given/When/Then)
- Assign priority (P0–P3)
- Show the card draft before any tracker write and wait for explicit approval
- Create/update tracker cards via the configured MCP/tooling with subtask checklists
- Fall back to structured draft-only mode when no writable tracker MCP exists
- Validate implemented solutions against acceptance criteria

**Triggers:** "product owner", "PO", "nova demanda", "criar issue", "refinar tarefa"

---

### 2. Architect (`architect`)

Analyzes issues from an architectural perspective.

**Responsibilities:**
- Identify affected architectural layers (per `docs/architecture.md`)
- Suggest file/module structure per layer
- Verify compliance with architectural boundaries
- Propose ADRs when needed
- When guidance is not already planned, request in the issue comment:
	- `documenter` guidance on how to document the demand
	- `test-advisor` guidance on high-level testing strategy
- Post architectural analysis as issue comment via MCP

"Already planned" means BOTH are true:
- relevant subtask exists in the issue checklist
- prior agent comment already requested/provided that guidance

The `architect` does not delegate execution. Delegation remains with `staff`.

**Triggers:** "architect", "análise arquitetural", "design técnico", "ADR"

---

### 3. Staff / Orchestrator (`staff`)

Central orchestrator that plans and coordinates implementation.

**Responsibilities:**
- Clarify ambiguities and validate issue quality before implementation starts
- Read PO context and Architect analysis from the issue
- Plan implementation at code level (files, order, dependencies)
- Document the plan on the issue via MCP
- Trigger `documenter` at task start for documentation impact mini-plan
- Consult `dba` when task includes database impact (schema/migrations/constraints/indexes/performance)
- Classify work as `feature_nova` or `mudanca_existente` before test planning
- Consult `test-advisor` for testing strategy based on classification
- Delegate to `backend-dev` and/or `frontend-dev` sub-agents
- Validate results and run tests
- Consult `metrifier` for observability recommendations
- Trigger `reviewer` before finalizing only when code changes exist
- Create branch and open PR via MCP

**Delegates to:** `backend-dev`, `frontend-dev`, `test-advisor`, `qa`, `metrifier`, `reviewer`, `documenter`, `dba`

**Triggers:** "staff", "orchestrator", "planejar implementação", "executar tarefa"

---

### 4. Backend Developer (`backend-dev`)

Implements backend code following the architectural style defined in `docs/architecture.md`. **Sub-agent only** (invoked by Staff).

**Responsibilities:**
- Implement domain entities, use cases, ports
- Implement repositories, controllers, DTOs, routes
- Wire dependencies in composition root
- Write unit and integration tests
- Consult `test-advisor` when needed

---

### 5. Frontend Developer (`frontend-dev`)

Implements frontend code using the framework defined in `CONTEXT_PACK.md`. **Sub-agent only** (invoked by Staff).

**Responsibilities:**
- Implement pages following project conventions
- Create components, hooks/equivalents, services
- Integrate with API per `docs/api-spec.md`
- Handle UI states (loading, error, empty, token expired)
- Write frontend tests

---

### 6. Test Advisor (`test-advisor`)

Proposes testing strategies without writing test code.

**Responsibilities:**
- Analyze tasks to identify all testable scenarios
- Consume task classification from Staff (`feature_nova` vs `mudanca_existente`)
- For `feature_nova`, propose new tests for new behavior
- For `mudanca_existente`, prefer adjusting existing tests when coverage is already sufficient
- Propose tests by pyramid level (unit → integration → e2e)
- Define mocking strategies and fixtures
- Identify edge cases and security test scenarios

**Triggers:** "test advisor", "quais testes", "estratégia de testes"

---

### 7. QA (`qa`)

Validates implementations by executing tests and verifying acceptance criteria.

**Responsibilities:**
- Execute automated test suites
- Validate acceptance criteria from the issue
- Test edge cases and security scenarios
- Post structured QA report on the issue via MCP

**Triggers:** "QA", "testar", "validar implementação", "rodar testes"

**Future:** Playwright MCP integration for browser-based testing

---

### 8. Reviewer (`reviewer`)

Reviews Pull Requests against project guidelines.

**Responsibilities:**
- Review code for architectural compliance per `docs/architecture.md`
- Validate API contract adherence
- Check security and observability practices
- Verify tests exist and cover changes
- Post structured review on PR via MCP

**Triggers:** "reviewer", "code review", "revisar PR"

---

### 9. Documenter (`documenter`)

Assesses and updates documentation needs from task start through completion.

**Responsibilities:**
- On every task start, produce a mandatory mini documentation plan (`required`, `optional`, `none`)
- Indicate docs likely impacted and recommended actions before implementation
- Analyze PR diff to identify doc-worthy changes
- Update affected docs (api-spec, database, domain, architecture, etc.)
- Create ADRs for architectural decisions
- Update CONTEXT_PACK.md for significant changes

**Triggers:** "documenter", "documentar", "atualizar docs", "criar ADR"

---

### 10. Metrifier (`metrifier`)

Suggests metrics and observability instrumentation.

**Responsibilities:**
- Propose business, technical, and operational metrics
- Recommend collection methods (structured logs, APM, custom)
- Suggest alert thresholds and dashboard structure
- Advise on SLI/SLO definitions

**Triggers:** "metrifier", "métricas", "observabilidade", "instrumentação"

---

### 11. Project Setup (`project-setup`)

First-use onboarding wizard that detects the environment, configures the project's technology stack, sets up tooling integrations, and validates Copilot readiness.

**Responsibilities:**
- Detect existing environment before asking questions (Phase 0: manifests, MCP servers, placeholders)
- Collect technology choices from the user (language, framework, database, APM, etc.)
- Collect tooling & integration preferences (issue tracker, MCP servers, doc hosting)
- Update all `[PREENCHER]` placeholders across docs and configuration
- Update `.vscode/mcp.json` with additional MCP servers if needed
- Adapt instruction files (`applyTo` patterns) to match the chosen stack
- Adapt agents if issue tracker is not GitHub Issues
- Validate that all setup-scope placeholders were replaced (post-apply check)
- Generate **Copilot Productivity Readiness** checklist as final output
- Suggest ADRs for key technical decisions
- Support idempotent re-runs (detect already-configured categories, skip or update)

**Triggers:** "setup project", "configurar projeto", "definir stack", "adaptar template", "project readiness", "check setup"

---

### 12. DBA (`dba`)

Owns database-related technical decisions for this repository.

**Responsibilities:**
- Analyze schema impact for tasks involving DB changes
- Recommend constraints, indexes, and migration safety patterns
- Validate rollback strategy (`up`/`down`) and integrity risks
- Keep recommendations database-agnostic unless docs define an engine-specific rule
- Trigger `documenter` when database documentation updates are required

**Triggers:** "dba", "database", "schema", "migration", "index", "constraint", "query performance"

---

### 13. Pathfinder (`pathfinder`)

Advisory agent that diagnoses uncertain or complex tasks and suggests the optimal agent workflow. First point of contact when you don't know where to start.

**Responsibilities:**
- Ask diagnostic questions to understand the task's nature, scope, and uncertainty
- Read existing cards/issues from any configured tracker to gather context
- Map tasks to the appropriate agents and their optimal execution order
- Suggest a high-level development roadmap inspired by agile workflows
- Identify risks, dependencies, and ambiguities before execution starts
- Explain why each agent should (or should not) be involved

**Triggers:** "pathfinder", "plan task", "planejar tarefa", "qual fluxo seguir", "por onde começar", "suggest workflow", "guide task", "não sei por onde começar"

---

## Delegation Model

```
User
 │
 ├── pathfinder ──→ Suggests agent workflow (optional first step)
 │
 ├── product-owner ──→ Drafts tracker card and creates/updates it after approval
 │
 ├── architect ──→ Posts architectural analysis on Issue (+ conditional requests to documenter/test-advisor)
 │
 ├── dba ──→ Owns database analysis and migration/constraint/index recommendations
 │
 ├── staff (orchestrator)
 │     ├── dba (when DB impact exists)
 │     ├── backend-dev ──→ Implements backend code
 │     │     └── test-advisor (consulta)
 │     ├── frontend-dev ──→ Implements frontend code
 │     │     └── test-advisor (consulta)
 │     ├── test-advisor ──→ Proposes test strategy
│     ├── metrifier ──→ Suggests metrics
│     ├── reviewer ──→ Reviews only when code changed
│     ├── documenter ──→ Mini-plan at task start (+ final doc validation)
│     └── qa ──→ Validates implementation
 │
├── reviewer ──→ Reviews PR when code changes exist
│
└── documenter ──→ Starts with doc mini-plan and closes with doc updates
```

---

## Tracker Card Protocol

All agents that interact with the configured tracker must keep the card updated:

1. **On start**: Post comment indicating the agent is acting
2. **Progress**: Update with what's been done and what remains
3. **Subtasks**: Use markdown checklists (`- [ ]`) to track progress
4. **Sub-issues**: Create linked sub-issues for large tasks
5. **On completion**: Post summary with PR links and subtask status

Standard format:
```markdown
## 🤖 [Agent Name] — Status Update
**Status**: 🟡 In progress / ✅ Completed / 🔴 Blocked

### Subtasks
- [x] Completed task
- [ ] Pending task

### Notes
[Details, decisions, blockers]
```

---

## Agent Execution Flows

> **Tip:** When the task is uncertain or you don't know where to start, use `pathfinder` first (`/plan-task`) to get a recommended workflow before entering any flow below.

### A) New Feature
```
(pathfinder) → product-owner → architect → dba(if DB changes) → staff(+documenter-start) → [backend-dev, frontend-dev] → qa → reviewer(code-change) → documenter(final)
```

### B) Bug Fix
```
(pathfinder) → product-owner (clarify) → dba(if DB changes) → staff(+documenter-start) → [backend-dev/frontend-dev] → qa → reviewer(code-change) → documenter(final)
```

### C) New Project (Bootstrap)
```
(pathfinder) → product-owner (backlog) → architect (structure) → staff(+documenter-start, scaffold) → documenter(final)
```

### D) Maintenance / Tech Debt
```
(pathfinder) → architect → staff(+documenter-start) → [backend-dev/frontend-dev] → reviewer(code-change) → documenter(final)
```

---

## Slash Commands (Prompts)

| Command | Agent | Purpose |
|---------|-------|---------|
| `/setup-project` | project-setup | Detect environment, configure stack, set up tooling, validate readiness |
| `/plan-task` | pathfinder | Diagnose a task and suggest the optimal agent workflow |
| `/new-feature` | product-owner | Start new feature flow |
| `/analyze-issue` | architect | Architectural analysis |
| `/analyze-database` | dba | Database analysis and migration/index/constraint guidance |
| `/implement-issue` | staff | Plan and implement |
| `/review-pr` | reviewer | Code review |
| `/fix-bug` | staff | Bug fix flow |
| `/document-pr` | documenter | Documentation workflow |

---

## Skills (Workflows)

| Skill | Purpose |
|-------|---------|
| `issue-triage` | Full triage pipeline: PO → Architect → Staff planning |
| `full-feature-cycle` | End-to-end: demand → merged, documented PR |

---

## Definition of Done

A task is complete when:
- [ ] Code follows the architectural style defined in `docs/architecture.md`
- [ ] API matches `docs/api-spec.md`
- [ ] Schema matches `docs/database.md`
- [ ] DBA validation completed when DB changes exist
- [ ] Tests follow strategy by classification (`feature_nova` or `mudanca_existente`)
- [ ] Logging includes `requestId`
- [ ] Security rules respected
- [ ] Documentation mini-plan was produced at task start
- [ ] Documentation updated if required by mini-plan or final diff
- [ ] Code review completed when code changed
- [ ] Tracker card updated with final status
- [ ] PR linked to issue

---

## Important Rules

Agents must **never**:
- Invent API endpoints
- Invent database tables or columns
- Bypass architectural layers
- Introduce undocumented behavior
- Log passwords, tokens, or PII

All changes must remain aligned with repository documentation.
