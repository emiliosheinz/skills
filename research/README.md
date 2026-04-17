# research

A Claude Code skill that deeply explores a problem space — interviewing the user, scanning the codebase, and researching external references — before any spec or design work begins.

## When to use

Invoke this skill when you want to:

- Understand a problem before writing requirements or designing a solution
- Gather context from the codebase, stakeholders, and external sources
- Produce a shared artifact that grounds subsequent PRD or technical design work
- Surface constraints, open questions, and prior art before committing to a direction

## How it works

The skill runs a sequential five-step process:

1. **Intake interview** — asks structured questions to capture the problem, affected users, current state, desired outcome, constraints, and what would make research complete. Does not read code or fetch URLs until this is done.
2. **Codebase scan** — scans existing modules, patterns, prior decisions in `.specs/`, and integration boundaries relevant to the problem. Records observations only — no conclusions.
3. **External research** — fetches and summarizes relevant documentation, prior art, competitor approaches, and standards surfaced in the previous steps.
4. **Synthesize and draft** — compiles all findings into a structured `RESEARCH.md` saved to `.specs/[feature-slug]/RESEARCH.md`.
5. **Recommend next steps** — presents the appropriate follow-on skill based on what the research revealed.

## Output

A `RESEARCH.md` file at `.specs/[feature-slug]/RESEARCH.md` containing:

- Problem framing from the user's perspective (current state, desired state, impact)
- Stakeholders and users with their primary needs
- Hard constraints (technical, legal, operational, time/team)
- Existing solutions and prior art
- Codebase findings (observations, not conclusions)
- External references with source URLs and key findings
- Open questions that must be resolved before spec work begins
- Recommended next steps

## Usage

```
/research
```

Claude will begin the intake interview immediately. Answer questions as specifically as possible — the more concrete the inputs, the more useful the research output. Vague answers will be followed up with targeted questions.
