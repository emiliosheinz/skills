# Agent Skills

A collection of reusable agent skills, tools, and workflows designed to extend LLM capabilities and enable autonomous task execution.

## Installation

Skills are installed using the [`skills` CLI](https://github.com/vercel-labs/skills).

**Install all skills:**

```bash
npx skills add emiliosheinz/agent-skills
```

**Install a single skill:**

```bash
npx skills add emiliosheinz/agent-skills --skill <skill-name>
```

By default, skills are installed locally to the current project. Use `--global` to install them to your user directory instead, making them available across all projects.

```bash
# Local (current project only)
npx skills add emiliosheinz/agent-skills

# Global (all projects)
npx skills add emiliosheinz/agent-skills --global
```

## Available Skills

| Skill | Description |
|-------|-------------|
| <nobr>`create-adr`</nobr> | Creates Architecture Decision Records (ADRs) to document significant architectural choices and their rationale for future team members. Use when the user says "create an ADR", "write an ADR", "create an ADR for X", "document a decision", "record why we chose X", or wants to capture the reasoning behind a technical choice so the team understands it later. Do NOT use for technical design, implementation plan, RFC, or PRD docs. |
| <nobr>`create-technical-design`</nobr> | Creates technical design documents covering architecture, system design, component responsibilities, data models, API contracts, trade-offs, and key decisions. Use when the user says "create a technical design", "write a technical design", "architecture document", "system design", "technical spec", or "design doc". Outputs architectural decisions only — no step-by-step implementation. Do NOT use for PRD, RFC, ADR, or implementation plans. |
| <nobr>`create-implementation-plan`</nobr> | Creates implementation plans covering phases, tasks, sequencing, dependencies, milestones, and risks. Use when the user says "create an implementation plan", "implementation plan", "execution plan", "plan the implementation of X", "how do we build X step by step", or "break this into phases". Can consume a technical design document as input. Do NOT use for architecture, system design, PRD, RFC, or ADR. |
| <nobr>`create-prd`</nobr> | Creates structured, explicit, and detailed Product Requirement Documents (PRDs). Use when the user says "create a PRD", "write a PRD", "create a PRD for X", "define the requirements for X", "write product requirements", or wants to plan what to build before implementation begins. Do NOT use for technical design, implementation plan, RFC, or ADR docs. |
| <nobr>`create-rfc`</nobr> | Creates structured Request for Comments (RFC) documents for proposing and deciding on significant changes. Use when the user says "create an RFC", "write an RFC", "create an RFC for X", "create a proposal", "draft an RFC", or needs stakeholder alignment before making a major technical or process decision. Do NOT use for technical design, implementation plan, PRD, or ADR docs. |
| <nobr>`implement`</nobr> | Executes implementation tasks by consuming existing PRD, technical design, and implementation plan artifacts. Use when the user says "implement this", "implement phase N", "build this feature", "start implementing", "execute the plan", or wants to turn requirements and design documents into working code using TDD (Red-Green-Refactor). Do NOT use for creating PRDs, technical designs, implementation plans, RFCs, or ADRs. |

## Document Workflow

The document skills map to different stages of the decision-to-implementation pipeline:

```
PRD ──→ RFC ──→ ADR ──→ Technical Design ──→ Implementation Plan ──→ Implement
What & Why    Which option?    Record decision    How it's structured    How to execute    Build it
```

| Stage | Skill | Question It Answers | When to Use |
|-------|-------|---------------------|-------------|
| 1. Requirements | `create-prd` | What are we building, for whom, and why? | Kicking off a new feature or initiative |
| 2. Decision | `create-rfc` | Should we do X or Y? Which approach? | Multiple viable options exist and stakeholders need to align |
| 3. Record | `create-adr` | Why did we choose X over Y? | After a significant architectural decision is made |
| 4. Design | `create-technical-design` | What is the architecture, data model, and API contract? | Before planning execution, to align the engineering team |
| 5. Plan | `create-implementation-plan` | How do we execute the work in phases? | After design is defined, to structure tasks and milestones |

Not every project needs all five stages. Common combinations:

- **Small feature**: PRD + technical design + implementation plan
- **Migration or large change**: RFC + ADR + technical design + implementation plan
- **Architectural decision only**: RFC + ADR (no implementation planning needed yet)
- **Full initiative**: PRD + RFC + ADR + technical design + implementation plan
