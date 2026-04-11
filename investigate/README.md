# investigate

A Claude Code skill that guides systematic debugging through a five-phase workflow: structured intake, evidence investigation, ranked hypothesis generation, test-case reproduction, and a concrete fix proposal saved as a persistent artifact.

## When to use

Invoke this skill when you want to:

- Debug an unexpected error, exception, or crash
- Investigate a failing test or broken behavior
- Diagnose an intermittent or hard-to-reproduce issue
- Produce a documented bug report with a verified root cause and fix plan

## How it works

The skill enforces a sequential five-phase flow with explicit gates between phases:

1. **Intake** — asks six structured questions to capture expected behavior, actual behavior, error messages, environment, reproducibility, and available evidence. Does not proceed until all questions are answered.
2. **Investigate** — reads the codebase to understand conventions, testing framework, and project structure before cataloguing all available evidence. No conclusions are formed yet.
3. **Hypothesize** — produces a ranked list of 3–5 root cause candidates, each with supporting evidence and a confidence level (High / Medium / Low).
4. **Verify** — works through hypotheses from highest to lowest confidence, writing a minimal test case in the project's established testing framework for each. Stops when one hypothesis is confirmed.
5. **Propose and save** — produces a structured bug report and saves it to `.specs/bugs/[bug-name].md`.

## Output

A Markdown file at `.specs/bugs/[bug-name].md` containing:

- Root cause with specific evidence (file, line, log line, or error text)
- Confidence level with justification
- Minimal reproduction test case following project conventions
- Numbered, actionable fix steps
- Prevention measures aligned with existing project tools and patterns

## Usage

```
/investigate
```

Claude will begin the intake questionnaire immediately. Answer each question as specifically as possible — include exact error messages, stack traces, and any recent changes to the environment.
