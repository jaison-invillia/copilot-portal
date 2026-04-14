---
description: "Use when working with project tracker cards/issues/tickets, updating task status, managing subtasks, tracking progress, creating child items, or maintaining work cards. Applies to agents that interact with the configured tracker via MCP or operate in draft-only mode."
---

# Tracker Card Protocol

This instruction defines the standard for how agents must keep project tracker cards updated during execution.

## Core principle

Every agent that interacts with the configured tracker via MCP must keep the work card updated with relevant progress, ensuring full visibility of what has been done and what remains.

If no writable tracker MCP is available, the agent must operate in **draft-only mode** and clearly state that no remote card was created or updated.

---

## Obligations

### 1. On start
Post a comment or tracker update indicating the agent is acting and what it will do.

### 2. Incremental progress
Update the card with what has been done and what remains.

### 3. Subtask checklists
Create or update a checklist of subtasks using tracker-native checklists when available, or markdown task lists (`- [ ]`) when supported, marking each item as it is completed.

### 4. Sub-issues
When a task is too large for a single card:
- Create child cards/sub-issues for major components when the tracker supports them
- Link to the parent using the tracker's native relationship model when available
- Reference all child items in the parent card description when possible

### 5. On completion
Post a final comment with:
- Summary of what was done
- Links to PRs/commits
- Status of all subtasks

### 6. Product Owner approval gate
Before creating or updating any tracker card, the `product-owner` must:
- Show the complete draft in the chat
- Wait for explicit user approval
- Create/update the remote card only after approval and only if writable tracker tooling exists
- Fall back to `READY - draft only` when writable tooling is unavailable

---

## Standard status update format

```markdown
## 🤖 [Agent Name] — Status Update

**Status**: 🟡 In progress / ✅ Completed / 🔴 Blocked

### Subtasks
- [x] Completed task
- [ ] Pending task
- [ ] Pending task

### Notes
[Relevant decisions, open questions, blockers, links to PRs]
```

---

## Status icons

| Icon | Meaning |
|------|---------|
| 🟡 | In progress — agent is actively working |
| ✅ | Completed — all subtasks done |
| 🔴 | Blocked — waiting for input or dependency |
| ➖ | Not applicable |

---

## Agent-specific expectations

| Agent | Creates cards? | Updates cards? | Creates child items? |
|-------|----------------|----------------|----------------------|
| product-owner | ✅ After explicit approval and only with writable MCP | ✅ After explicit approval and only with writable MCP | ✅ When task is large and tracker supports it |
| architect | ❌ No | ✅ Comments/notes only on approved cards when supported (including conditional requests to `documenter`/`test-advisor` when guidance is not already planned) | ❌ No |
| dba | ❌ No | ✅ Database analysis comments on approved cards when DB impact exists | ❌ No |
| staff | ❌ No | ✅ Yes on approved cards (plan + progress + ambiguity/doc triage) | ✅ When delegating work and tracker supports it |
| qa | ❌ No | ✅ Test results on approved cards | ❌ No |
| reviewer | ❌ No | ✅ Review results (only when code changed) | ❌ No |
| documenter | ❌ No | ✅ Mini-plan at task start + final doc summary on approved cards | ❌ No |

---

## Subtask checklist template (for Product Owner)

When creating a tracker card, include a standard subtask checklist when the tracker supports it:

```markdown
## Subtasks
- [ ] 🏗️ Architectural analysis (@architect)
- [ ] 🗄️ Database analysis when DB impact exists (@dba)
- [ ] 🧭 Documentation guidance covered in architecture phase (subtask + agent comment) when applicable
- [ ] 🧭 High-level testing guidance covered in architecture phase (subtask + agent comment) when applicable
- [ ] 🛠️ Implementation plan (@staff)
- [ ] ❓ Ambiguity and quality check (@staff)
- [ ] 📝 Documentation mini-plan at task start (@documenter)
- [ ] 💻 Backend implementation (@backend-dev)
- [ ] 🎨 Frontend implementation (@frontend-dev)
- [ ] 🧪 Testing strategy by classification (@test-advisor)
- [ ] ✅ QA validation (@qa)
- [ ] 📊 Metrics recommendation (@metrifier)
- [ ] 🔍 Code review (@reviewer, only if code changes)
- [ ] 📝 Documentation update (@documenter)
```

Not all subtasks apply to every card. The Product Owner should include only relevant ones.

If the tracker is unavailable for writing, return the same structure in the draft shown to the user and explicitly mark it as manual-creation content.
