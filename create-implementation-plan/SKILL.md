---
name: create-implementation-plan
description: Creates implementation plans covering phases, tasks, sequencing, dependencies, milestones, and risks. Use when the user says "create an implementation plan", "implementation plan", "execution plan", "plan the implementation of X", "how do we build X step by step", or "break this into phases". Can consume a technical design document as input. Do NOT use for architecture, system design, PRD, RFC, or ADR.
---

# Implementation Plan Creator

Creates implementation plans that define how to execute a project — phases, tasks, sequencing, testing, and milestones. Does not define system architecture. References a technical design when one exists.

## Core Principle: Vertical Slices

Structure every plan as **vertical slices** following the **tracer bullet strategy**. Each phase delivers a thin but complete slice through all layers.

- Phase 1 is always the tracer bullet: the simplest possible end-to-end flow that proves the architecture works
- Every phase produces a deployable increment that can be demonstrated and validated
- Testing is embedded in each phase — never deferred to a standalone phase
- No phase is purely "setup" or "infrastructure" without user-visible functionality

### Phase Structure

Each phase must define:

- **Goal**: User-facing or system-visible capability this phase delivers
- **Vertical slice**: Which layers are touched and what flows end-to-end
- **Tasks**: Specific work items with owners and estimates
- **Testing**: What is tested in this phase and how
- **Acceptance criteria**: How to verify the phase goal
- **Dependencies**: What must be complete before this phase starts

### Example Phase Progression

- **Phase 1 (Tracer Bullet)**: Single entity, one endpoint, basic persistence, one happy-path test — proves the full stack end-to-end
- **Phase 2**: Core business logic, error handling, validation — primary use case complete
- **Phase 3**: External integrations, async processing — system connects to the outside world
- **Phase 4**: Edge cases, advanced features, performance — production hardening
- **Phase N (Final)**: Phased rollout, monitoring verification, documentation — production readiness

## Skill Routing

| Signal | Skill |
|--------|-------|
| Define what to build and why | `/create-prd` |
| Define architecture, components, APIs | `/create-technical-design` |
| Phases, tasks, sequencing, milestones | **This skill** |
| Turn plans into working code | `/implement` |

## Process

### Step 1: Check for Technical Design

Ask the user:
- Does a technical design document already exist?
- If yes, request it or the file path (e.g. `.specs/[feature-slug]/TECHNICAL-DESIGN.md`), then read it.

If a technical design is provided, use it as the source of truth for architecture, components, APIs, and constraints. Reference it explicitly throughout the plan. Do not re-derive architectural decisions — cite the design.

### Step 2: Gather Context

Ask for:
- Project/feature name and team members
- Target timeline or milestone dates
- Any known constraints (hard deadlines, team availability, external dependencies)

### Step 3: Collect Phase Information

Ensure you have enough to structure phases with vertical slices:
- What is the simplest end-to-end flow that proves the architecture?
- What is the core business logic that must be complete for V1?
- Are there external integrations or async processes?
- What does production readiness require (monitoring, rollback, docs)?

### Step 4: Determine Critical Sections

| Section | Required When |
|---------|---------------|
| **Testing Strategy** | All projects |
| **Rollback Plan** | Production deployments |

**Testing Strategy**: test types (unit, integration, e2e, contract, load) with scope, approach, and critical scenarios per type. Each phase embeds its own testing — this section describes the overall strategy and test data management approach.

**Rollback Plan**: deployment strategy (feature flags, phased rollout percentages), rollback triggers with specific thresholds, rollback steps (immediate, database, communication), post-rollback process (RCA, fix, re-test, re-deploy).

### Step 5: Generate and Validate

**File location**: `.specs/[feature-slug]/IMPLEMENTATION-PLAN.md` — derive the slug from the feature name (lowercase, hyphen-separated). Create the directory if it does not exist.

Generate using the template below. Validate against the checklist. Highlight any gaps in the output summary.

## Template

```markdown
# Implementation Plan — [Project/Feature Name]

| Field        | Value                        |
| ------------ | ---------------------------- |
| Tech Lead    | @Name                        |
| Team         | Name1, Name2, Name3          |
| Epic/Ticket  | [Link]                       |
| Status       | Draft / In Review / Approved |
| Created      | YYYY-MM-DD                   |
| Last Updated | YYYY-MM-DD                   |

## Overview

Brief description of what is being built and the execution strategy.

**Technical Design**: [link to TECHNICAL-DESIGN.md if available, or "Not provided"]

## Implementation Phases

Structured as vertical slices following tracer bullet strategy. Testing is embedded in every phase.

### Phase 1: [Tracer Bullet — simplest end-to-end flow]

**Goal**: [User-facing or system-visible capability this phase delivers]

**Vertical slice**: [Which layers are touched and what flows end-to-end]

**Tasks**:

| Task | Owner | Estimate |
|------|-------|----------|
| [Task 1] | @Name | [estimate] |
| [Task 2] | @Name | [estimate] |

**Testing**:

- [Test type]: [Specific scenarios verified in this phase]

**Acceptance Criteria**:

- [ ] [Verifiable outcome 1]
- [ ] [Verifiable outcome 2]

**Dependencies**: [What must be complete before this phase starts]

---

### Phase 2: [Core business logic complete]

[Same structure as Phase 1]

---

### Phase N: [Production readiness]

[Same structure as Phase 1]

## Milestones

| Milestone | Target Date | Description |
|-----------|-------------|-------------|
| [Name] | YYYY-MM-DD | [What is delivered at this point] |

## Dependencies

| Dependency | Type | Owner | Status | Risk if Delayed |
|------------|------|-------|--------|-----------------|
| [Item] | External / Internal / Technical | @Owner | [Status] | [Impact] |

## Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| [Description] | H/M/L | H/M/L | [Strategy] |
```

Critical sections (Testing Strategy, Rollback Plan) follow after Risks when applicable.

## Validation Checklist

- [ ] Technical design referenced (if one exists)
- [ ] Phase 1 is a tracer bullet delivering end-to-end value
- [ ] Every phase has goal, tasks, testing, acceptance criteria, and dependencies
- [ ] Testing is embedded in each phase — no standalone test phase
- [ ] No phase is purely setup or infrastructure without user-visible output
- [ ] Milestones are defined with target dates
- [ ] Dependencies listed with owners and delay risk
- [ ] Risks has 3+ entries with impact, probability, and mitigation
- [ ] Testing Strategy present (all projects)
- [ ] Rollback Plan present (production deployments)

## Output Summary

After generating, provide:

```
Implementation Plan Created: "[Project Name]"
File: .specs/[feature-slug]/IMPLEMENTATION-PLAN.md

Phases: N
Estimated Total Duration: [sum of estimates]

Technical Design: [Linked / Not provided]

Sections Included:
- Mandatory: [list]
- Critical (N/N): [list]

Missing (if any):
- [Section]: [why it matters]

Suggested Next Steps:
- Review phases with team
- Break tasks into tickets/issues
- Run /implement to begin execution
```
