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
| <nobr>`create-adr`</nobr> | Creates Architecture Decision Records (ADRs) to document significant architectural choices and their rationale for future team members. Use when the user says "create an ADR", "write an ADR", "create an ADR for X", "document a decision", "record why we chose X", or wants to capture the reasoning behind a technical choice so the team understands it later. Do NOT use for TDD, RFC, or PRD docs. |
| <nobr>`create-prd`</nobr> | Creates structured, explicit, and detailed Product Requirement Documents (PRDs). Use when the user says "create a PRD", "write a PRD", "create a PRD for X", "define the requirements for X", "write product requirements", or wants to plan what to build before implementation begins. Do NOT use for TDD, RFC, or ADR docs. |
| <nobr>`create-rfc`</nobr> | Creates structured Request for Comments (RFC) documents for proposing and deciding on significant changes. Use when the user says "create an RFC", "write an RFC", "create an RFC for X", "create a proposal", "draft an RFC", or needs stakeholder alignment before making a major technical or process decision. Do NOT use for TDD, PRD, or ADR docs. |
| <nobr>`create-tdd`</nobr> | Creates Technical Design Documents (TDD) through interactive discovery. Use when the user says "create a TDD", "write a TDD", "create a TDD for X", "write a design doc", "technical spec", "architecture document", or "design proposal". Do NOT use for PRD, RFC, or ADR docs. |

## Document Workflow

The four document skills map to different stages of the decision-to-implementation pipeline:

```
PRD ──→ RFC ──→ ADR ──→ TDD
What & Why    Which option?    Record the decision    How to build it
```

| Stage | Skill | Question It Answers | When to Use |
|-------|-------|---------------------|-------------|
| 1. Requirements | `create-prd` | What are we building, for whom, and why? | Kicking off a new feature or initiative |
| 2. Decision | `create-rfc` | Should we do X or Y? Which approach? | Multiple viable options exist and stakeholders need to align |
| 3. Record | `create-adr` | Why did we choose X over Y? | After a significant architectural decision is made |
| 4. Design | `create-tdd` | How do we build it? | Before implementation, to align the engineering team |

Not every project needs all four documents. Common combinations:

- **Small feature**: PRD + TDD (no significant decisions to debate)
- **Migration or large change**: RFC + ADR + TDD (direction is unclear, needs alignment)
- **Architectural decision**: RFC + ADR (no implementation planning needed yet)
- **Full initiative**: PRD + RFC + ADR + TDD (new product area with significant technical decisions)
