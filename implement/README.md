# implement

An agent skill that executes implementation tasks by consuming existing PRD, technical design, and implementation plan artifacts, translating requirements into working, tested code through strict Test-Driven Development (Red-Green-Refactor).

## When to use

Invoke this skill when you want to:

- Turn defined requirements (PRD), technical design, and implementation plan into working code
- Implement a specific phase from an implementation plan
- Execute incremental, test-driven development with strong feedback loops
- Deliver code that is validated against existing specifications

## How it works

The skill runs a seven-step process:

1. **Understand the context** — reads the PRD, technical design, and implementation plan; identifies scope, constraints, and acceptance criteria for the task.
2. **Break the phase into tasks** — if the phase contains multiple distinct behaviors, breaks it into ordered tasks, each completable in a single TDD cycle.
3. **Execute (Red-Green-Refactor)** — for each slice, writes a failing test, implements minimal code to pass, refactors while green. Runs tests at every step.
4. **Review and validate** — runs the full test suite, linters, and formatters. Verifies alignment with PRD requirements and technical design architecture.
5. **Summarize and request feedback** — presents what was implemented, which requirements were covered, decisions made, and open questions.
6. **Iterate** — incorporates user feedback by repeating the TDD cycle for affected slices.
7. **Commit (manual gate)** — proposes a commit message and file list. Never commits without explicit user approval.

## Output

Tested, working code that satisfies the requirements defined in the PRD and follows the architecture defined in the technical design. The skill also produces:

- A task breakdown of the phase (before coding, when needed)
- A summary of covered requirements and decisions made (after coding)
- A proposed commit message (when the user is satisfied)

## Usage

```
/implement
```

The agent begins by locating and reading the relevant PRD, technical design, and implementation plan. Specify the feature slug or phase if the project has multiple features or phases — otherwise the agent will ask.
