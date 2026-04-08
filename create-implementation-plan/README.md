# create-implementation-plan

An agent skill that guides the creation of implementation plans covering phases, tasks, sequencing, dependencies, milestones, and risks. Works independently or as a follow-up to a technical design document.

## When to use

Invoke this skill when you want to:

- Break a feature or project into structured, executable phases
- Define tasks, ownership, sequencing, and milestones for the engineering team
- Produce an implementation plan that references an existing technical design
- Capture risks, testing strategy, and rollback approach before execution begins

## How it works

The skill runs a five-step process:

1. **Check for technical design** — asks if a technical design exists and reads it if provided, using it as the source of truth for architecture and constraints.
2. **Gather context** — collects project name, team, timeline, and known constraints.
3. **Collect phase information** — ensures enough detail exists to structure phases as vertical slices following the tracer bullet strategy.
4. **Determine critical sections** — identifies required sections based on project type (testing strategy for all projects, rollback plan for production deployments).
5. **Generate and validate** — produces a structured Markdown plan, validates it against a checklist, and highlights any gaps.

## Output

A Markdown implementation plan written to `.specs/[feature-slug]/IMPLEMENTATION-PLAN.md` containing:

- Header and metadata (tech lead, team, epic link, status, dates)
- Overview — brief description linking to the technical design if available
- Implementation phases — structured as vertical slices (tracer bullet strategy), each with goal, tasks, testing, acceptance criteria, and dependencies
- Milestones — named checkpoints with target dates
- Dependencies — external, internal, and technical dependencies with owners and delay risk
- Risks — minimum 3 risks with impact, probability, and mitigation

Critical sections (testing strategy, rollback plan) are included based on project type.

## Usage

```
/create-implementation-plan
```

The agent will ask whether a technical design document exists before proceeding. Provide as much context as possible about team structure, timeline, and constraints — the more context, the more actionable the plan.

## Workflow position

```
PRD ──→ RFC ──→ ADR ──→ Technical Design ──→ Implementation Plan ──→ Implement
```

This skill consumes the output of `/create-technical-design` and produces an execution plan that `/implement` can act on.
