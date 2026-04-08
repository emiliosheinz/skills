# create-technical-design

An agent skill that guides the creation of technical design documents covering system architecture, component design, data models, API contracts, trade-offs, and key decisions.

## When to use

Invoke this skill when you want to:

- Document the architecture and technical approach for a feature or integration before implementation begins
- Define component responsibilities, API contracts, and data models for the team to align on
- Capture key decisions, trade-offs, and risks in a reviewable artifact
- Produce a technical design that an implementation plan can reference and execute against

## How it works

The skill runs a five-step process:

1. **Gather context** — collects project name, type, and whether a clear problem statement exists.
2. **Collect mandatory information** — ensures tech lead, team, problem description, scope, solution approach, key decisions, and risks are present before proceeding.
3. **Determine critical sections** — based on project type, identifies required additional sections (security for payment/auth/PII projects, monitoring for production systems).
4. **Offer suggested sections** — based on project size, offers optional sections such as alternatives considered, performance requirements, and open questions.
5. **Generate and validate** — produces a structured Markdown design document, validates it against a checklist, and highlights any gaps.

## Output

A Markdown technical design written to `.specs/[feature-slug]/TECHNICAL-DESIGN.md` containing:

- Header and metadata (tech lead, team, epic link, status, dates)
- Context — background, domain, stakeholders
- Problem statement and motivation — specific problems with quantified impact, why now, cost of inaction
- Scope — in-scope (V1), out-of-scope, future considerations
- Technical solution — architecture diagram, key components, data flow, API contracts, database changes
- Key decisions and trade-offs — choices made and alternatives rejected with rationale
- Risks — minimum 3 risks with impact, probability, and mitigation

Critical and suggested sections are included based on project type and size.

## Usage

```
/create-technical-design
```

The agent begins gathering context immediately. Provide as much detail as possible about the problem and the technical approach — the more context, the more useful the design will be to reviewers.

## Workflow position

```
PRD ──→ RFC ──→ ADR ──→ Technical Design ──→ Implementation Plan ──→ Implement
```

The output of this skill feeds directly into `/create-implementation-plan`, which plans the execution phases that build what this design describes.
