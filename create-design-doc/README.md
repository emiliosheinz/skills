# create-design-doc

An agent skill that guides the creation of design documents to communicate architecture decisions, implementation plans, and risk assessments before implementation begins.

## When to use

Invoke this skill when you want to:

- Document the architecture and technical approach for a new feature or integration
- Align the team on implementation strategy before writing code
- Capture risks, trade-offs, and mitigation plans for a project
- Create a reviewable artifact for stakeholder approval on significant technical work

## How it works

The skill runs a five-step process:

1. **Gather context** — collects project name, size, type, and whether the user has a clear problem statement.
2. **Collect mandatory information** — ensures tech lead, team, problem description, scope, solution approach, risks, and implementation plan are present before proceeding.
3. **Determine critical sections** — based on project type, identifies required additional sections (security for payment/auth, monitoring and rollback for production systems, testing for all projects).
4. **Offer suggested sections** — based on project size, offers optional sections like success metrics, alternatives considered, migration plan, and open questions.
5. **Generate and validate** — produces a structured Markdown design doc, validates it against a checklist, and highlights any gaps.

## Output

A Markdown design doc written to `.specs/[feature-slug]/DESIGN.md` containing:

- Header and metadata (tech lead, team, epic link, status, dates)
- Context — background, domain, stakeholders
- Problem statement and motivation — specific problems with quantified impact, why now, cost of inaction
- Scope — in-scope (V1), out-of-scope, future considerations
- Technical solution — architecture diagram, key components, data flow, API contracts, database changes
- Risks — minimum 3 risks with impact, probability, and mitigation
- Implementation plan — vertical slices following tracer bullet strategy, each phase delivering end-to-end value

Critical and suggested sections are included based on project type and size.

## Usage

```
/create-design-doc
```

The agent begins gathering context immediately. Provide as much detail as possible about the project, the problem being solved, and the technical approach — the more context, the more useful the design doc will be to reviewers.
