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
