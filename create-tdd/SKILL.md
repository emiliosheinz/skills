---
name: create-tdd
description: Creates Technical Design Documents (TDD) through interactive discovery. Use when the user says "create a TDD", "write a TDD", "create a TDD for X", "write a design doc", "technical spec", "architecture document", or "design proposal". Do NOT use for PRD, RFC, or ADR docs.
---

# TDD Creator

You create Technical Design Documents that communicate architecture decisions, implementation plans, and risk assessments. TDDs document **architectural decisions and contracts**, not implementation code.

## TDD vs PRD

| Aspect | TDD | PRD |
|--------|-----|-----|
| **Purpose** | Design + plan implementation | Define what to build and why |
| **Audience** | Engineering team | Product team, engineering team |
| **Focus** | How do we build X? | What are we building, for whom, and why? |
| **Output** | Architecture + implementation plan | Feature requirements, user stories |
| **Timing** | After requirements are defined | Before implementation, during planning |

Use TDD when you need to document the **technical approach**. Use PRD when you need to **define the product requirements**.

## TDD vs RFC

| Aspect | TDD | RFC |
|--------|-----|-----|
| **Purpose** | Design + plan implementation | Propose + decide on significant change |
| **Audience** | Engineering team | Broad stakeholders, leadership |
| **Focus** | How do we build X? | Should we do X? Which option? |
| **Output** | Architecture + implementation plan | Decision + rationale |
| **Timing** | After direction is decided | Before committing to a direction |

Use TDD when the decision is made and you need to document the **implementation approach**. Use RFC when the **decision itself** needs alignment.

## TDD vs ADR

| Aspect | TDD | ADR |
|--------|-----|-----|
| **Purpose** | Design + plan implementation | Record a finalized architectural decision |
| **Audience** | Engineers implementing the solution | Future engineers, architects |
| **Focus** | How do we build X? | Why did we choose X over Y? |
| **Output** | Architecture + implementation plan | Decision + consequences |
| **Timing** | During implementation planning | After a significant decision is made |

Use TDD when you need to document **how to implement**. Use ADR when you need to **record why a decision was made** for future reference.

## Core Principle: Architecture Over Implementation

Before adding any detail, ask: "If we change frameworks, does this still apply?"

- YES: Include it (architectural decision)
- NO: Exclude it (implementation detail)

**Include**: API contracts, data schemas, component diagrams, technology decisions with rationale, rollback strategies, log/event schemas.

**Exclude**: CLI commands, code snippets, framework-specific syntax, file paths, tool-specific configuration.

The TDD must survive implementation changes. If the team migrates frameworks or ORMs, the TDD should remain valid.

## Document Sections

### Mandatory (always required)

If the user does not provide information for these, ask using "AskUserQuestion".

1. **Header & Metadata** -- Project name, tech lead, team, epic/ticket link, status, dates
2. **Context** -- Background (current state), business domain, stakeholders
3. **Problem Statement & Motivation** -- Specific problems with quantified impact, why now, cost of inaction
4. **Scope** -- In-scope items (V1/MVP), out-of-scope items, future considerations (V2+). Minimum 3 items each for in/out of scope.
5. **Technical Solution** -- Architecture overview with diagram (Mermaid/PlantUML), key components and responsibilities, data flow, API contracts (endpoints, methods, request/response schemas), database changes (new tables, schema modifications, migration strategy, indexes)
6. **Risks** -- Minimum 3 risks. Each with: description, impact (High/Medium/Low), probability (High/Medium/Low), mitigation strategy.
7. **Implementation Plan** -- Phases structured as vertical slices following tracer bullet strategy (see Implementation Plan Guidelines below)

### Critical (required based on project type)

Determine applicability from project context. If applicable but missing, ask the user using "AskUserQuestion".

| Section                        | Required When                          |
| ------------------------------ | -------------------------------------- |
| **Security Considerations**    | Payments, auth, PII, external integrations |
| **Testing Strategy**           | All projects (highly recommended)      |
| **Monitoring & Observability** | Production systems                     |
| **Rollback Plan**              | Production deployments                 |

**Security Considerations** covers: authentication/authorization approach, encryption (at rest, in transit), PII handling and retention, compliance requirements (GDPR, PCI DSS, etc.), secrets management and rotation policy, webhook signature validation.

**Testing Strategy** covers: test types with scope and approach (unit, integration, e2e, contract, load), critical test scenarios per type, test data management approach.

**Monitoring & Observability** covers: key metrics with alert thresholds, structured logging format and what to log/not log (never log secrets or full card numbers), alert severity levels and response actions, operational and business dashboards.

**Rollback Plan** covers: deployment strategy (feature flags, phased rollout percentages), rollback triggers with specific thresholds, rollback steps (immediate, database, communication), post-rollback process (RCA, fix, re-test, re-deploy).

### Suggested (offer based on project size)

After mandatory and critical sections are covered, offer these. For small projects and simple projects, skip most. For large and complex projects, include all that are relevant.

| Section                    | When Valuable                               |
| -------------------------- | ------------------------------------------- |
| **Success Metrics**        | Business KPIs, adoption targets             |
| **Alternatives Considered**| Multiple viable approaches existed           |
| **Dependencies**           | External services, team dependencies, blockers |
| **Performance Requirements**| Latency/throughput/availability targets     |
| **Migration Plan**         | Replacing existing systems                  |
| **Open Questions**         | Unresolved decisions blocking implementation |
| **Glossary**               | Domain-specific terminology                 |
| **Approval & Sign-off**    | Requires stakeholder approval               |

## Implementation Plan Guidelines

Structure every implementation plan as **vertical slices** following the **tracer bullet strategy**. Each phase must deliver incremental, testable, end-to-end value.

### Vertical Slice Principles

- Each phase delivers a thin but complete slice through all layers (data, service, API, UI if applicable)
- Phase 1 is always the tracer bullet: the simplest possible end-to-end flow that proves the architecture works
- Every phase produces a deployable increment that can be demonstrated and validated
- Testing is embedded in each phase, not deferred to a separate phase
- No phase should be purely "setup" or "infrastructure" without delivering user-visible functionality

### Phase Structure

Each phase must define:

- **Goal**: What user-facing or system-visible capability this phase delivers
- **Vertical slice**: Which layers are touched and what flows end-to-end
- **Tasks**: Specific work items with owners and estimates
- **Acceptance criteria**: How to verify the phase delivers its goal
- **Dependencies**: What must be complete before this phase starts

### Example Phase Progression

- **Phase 1 (Tracer Bullet)**: Single entity, one API endpoint, basic persistence, one happy-path test -- proves the full stack works end-to-end
- **Phase 2**: Add core business logic, error handling, validation -- the primary use case works completely
- **Phase 3**: External integrations, webhooks, async processing -- system connects to the outside world
- **Phase 4**: Edge cases, advanced features, performance optimization -- production hardening
- **Phase N (Final)**: Phased rollout, monitoring verification, documentation -- production readiness

Each phase includes its own testing. Never create a standalone "testing phase."

## Process

Follow these steps when creating a TDD:

### Step 1: Gather Context

Ask the user for:
- Project/feature name
- Project size (small: < 1 week, medium: 1-4 weeks, large: > 1 month)
- Project type (new feature, integration, refactor/migration, infrastructure, payment/billing, auth)
- Whether they have a clear problem statement or need help defining one

### Step 2: Collect Mandatory Information

Based on answers, ensure you have enough to write all mandatory sections. Ask for anything missing:
- Tech lead and team members
- Problem description with impact (what, why, quantified consequences)
- In-scope and out-of-scope items
- High-level solution approach
- At least 3 risks
- Enough detail to plan implementation phases

### Step 3: Determine Critical Sections

Based on project type, identify which critical sections are required and ask for missing information:
- Payment/auth/PII projects: Security is mandatory
- Production systems: Monitoring and Rollback are mandatory
- All projects: Testing Strategy is strongly recommended

### Step 4: Offer Suggested Sections

Based on project size, offer relevant suggested sections. Ask: "Would you like to include any of these sections?"

### Step 5: Generate and Validate

**File location:** Write the TDD to `.specs/[feature-slug]/TDD.md`. Derive the slug from the feature name: lowercase, words separated by hyphens (e.g. `stripe-integration`, `user-onboarding`). Create the directory if it does not exist. This co-locates the TDD with related documents for the same initiative.

Generate the TDD using the template below, then validate against the checklist. Highlight any gaps in a summary.

## TDD Template

```markdown
# TDD - [Project/Feature Name]

| Field        | Value                        |
| ------------ | ---------------------------- |
| Tech Lead    | @Name                        |
| Team         | Name1, Name2, Name3          |
| Epic/Ticket  | [Link]                       |
| Status       | Draft / In Review / Approved |
| Created      | YYYY-MM-DD                   |
| Last Updated | YYYY-MM-DD                   |

## Context

2-4 paragraphs covering: current state, what system/feature this relates to, business domain, and who the stakeholders are (users, business, compliance, etc.).

## Problem Statement & Motivation

### Problems We're Solving

- **[Problem 1]**: [Specific pain point]
  - Impact: [Quantify: time wasted, cost, user friction, revenue loss]
- **[Problem 2]**: [Specific pain point]
  - Impact: [Quantify]

### Why Now?

- [Business, technical, or user driver]

### Cost of Inaction

- **Business**: [Revenue loss, competitive disadvantage]
- **Technical**: [Debt accumulation, system degradation]
- **Users**: [Poor experience, churn risk]

## Scope

### In Scope (V1)

- [Capability 1]
- [Capability 2]
- [Capability 3]

### Out of Scope (V1)

- [Item 1] (deferred to V2)
- [Item 2] (not needed for MVP)
- [Item 3] (future enhancement)

### Future Considerations (V2+)

- [Item dependent on V1 validation]

## Technical Solution

### Architecture Overview

[High-level description of the solution]

**Key Components**:

- [Component A]: [responsibility]
- [Component B]: [responsibility]

**Architecture Diagram**:

[Mermaid or PlantUML diagram showing component interactions]

### Data Flow

1. [Step 1]: [Source] -> [Destination] ([protocol/method])
2. [Step 2]: [Source] -> [Destination] ([protocol/method])

### APIs & Endpoints

| Endpoint | Method | Description | Request | Response |
|----------|--------|-------------|---------|----------|
| [path]   | [verb] | [what it does] | [schema/DTO] | [schema/status] |

### Database Changes

**New Tables**:

- [TableName] - [description]
  - Fields: [list primary fields with types]
  - Indexes: [list indexes with rationale]

**Schema Modifications** (if any):

- [Change description with type, constraints, migration strategy]

## Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| [Description] | High/Medium/Low | High/Medium/Low | [Specific mitigation strategy] |

Minimum 3 risks. Consider: external dependencies, data integrity, performance, security, scope changes.

## Implementation Plan

Structured as vertical slices following tracer bullet strategy. Each phase delivers end-to-end value. Testing is embedded in every phase.

| Phase | Goal | Tasks | Owner | Estimate |
|-------|------|-------|-------|----------|
| **Phase 1** | [Simplest end-to-end flow proving the architecture] | [Task list] | @Name | [estimate] |
| **Phase 2** | [Core business logic complete] | [Task list] | @Name | [estimate] |
| **Phase N** | [Production readiness] | [Task list] | @Name | [estimate] |

**Dependencies**: [What must be complete before each phase]
```

The sections below are included or omitted based on project type and size. When included, add them after the Implementation Plan.

**Security Considerations** (payment/auth/PII projects): authentication/authorization approach, encryption at rest and in transit, PII handling and retention, compliance requirements, secrets management, webhook signature validation.

**Testing Strategy** (all projects): test types table (type, scope, approach), critical test scenarios per type, test data management.

**Monitoring & Observability** (production systems): metrics table (metric, type, alert threshold), structured log format, what to log and what never to log, alert severity and response actions.

**Rollback Plan** (production deployments): deployment strategy with feature flags and rollout percentages, rollback triggers with thresholds, rollback steps (immediate, database, communication), post-rollback process.

**Alternatives Considered**: comparison table (option, pros, cons, why not chosen), decision criteria with weights, rationale for chosen options

**Other suggested sections** (Success Metrics, Dependencies, Performance Requirements, Migration Plan, Open Questions, Glossary, Approval & Sign-off): include when relevant based on project size and context. Follow the same pattern of tables and structured lists.

## Validation Checklist

Before finalizing, verify:

**Mandatory sections**:
- [ ] Header has tech lead, team, and epic link
- [ ] Context has 2+ paragraphs with background and domain
- [ ] Problem statement has 2+ specific problems with quantified impact
- [ ] Scope has 3+ in-scope and 3+ out-of-scope items
- [ ] Technical solution has architecture diagram and API contracts
- [ ] Risks has 3+ entries with impact, probability, and mitigation
- [ ] Implementation plan uses vertical slices (no standalone testing or setup phases)

**Critical sections (when applicable)**:
- [ ] Security: auth method, encryption, PII handling, compliance (payment/auth projects)
- [ ] Monitoring: 3+ metrics with thresholds, alerts configured (production systems)
- [ ] Rollback: triggers and steps defined (production deployments)
- [ ] Testing: 2+ test types with critical scenarios (all projects)

## Output Summary

After generating the TDD, provide a summary:

```
TDD Created: "[Project Name]"

Sections Included:
- Mandatory (N/7): [list]
- Critical (N/N): [list]
- Suggested: [list]

Missing (if any):
- [Section]: [why it matters]

Suggested Next Steps:
- [Actionable items like adding missing sections, scheduling review, creating tickets]
```
