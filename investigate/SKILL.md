---
skill: investigate
description: >
  Guides the agent through a five-phase debugging workflow: structured intake,
  evidence investigation, ranked hypothesis generation, test-case reproduction,
  and a concrete fix proposal with prevention measures. Saves the final report
  to .specs/bugs/[bug-name].md following project conventions.
triggers: debug, bug, error, not working, broken, issue, exception, crash, failing test, unexpected behavior
---

# Investigation Executor

**Goal:** identify and verify the root cause of a issue through systematic investigation, reproduce it with a minimal test case following project conventions, and produce a concrete fix proposal saved as a persistent artifact.

## Routing

- Building a new feature → `/create-prd`
- Proposing a significant change → `/create-rfc`
- Documenting an architectural decision → `/create-adr`

## Progress Checklist

Track your phase as you work. Update this mentally — never skip ahead:

- [ ] Phase 1 — Intake complete (all six questions answered)
- [ ] Phase 2 — Investigation complete (evidence catalogued, no conclusions yet)
- [ ] Phase 3 — Hypotheses ranked (3–5 candidates with confidence levels)
- [ ] Phase 4 — Hypothesis verified (reproduction confirmed, fix not yet proposed)
- [ ] Phase 5 — Report saved to `.specs/bugs/[bug-name].md`

## Phase 1 — Intake

**Do not read any code or form any theory yet.**

Ask the user all six questions before proceeding. Use the `AskUserQuestion` tool to present them. Accept "unknown" as a valid answer, but record it explicitly.

1. What did you expect to happen?
2. What is actually happening? (include any error messages verbatim)
3. When did this start? Did anything change recently — deploy, dependency update, config change?
4. What is your environment? (language, framework/runtime, version, OS)
5. Is this reproducible consistently, or intermittent?
6. Can you share the relevant code, logs, or stack trace?

**Gate:** Do not proceed to Phase 2 until all six questions are answered or marked "unknown".

## Phase 2 — Investigate

**This phase is purely observational. Make no conclusions.**

Read the codebase to understand its conventions before examining the evidence:

1. Scan for project documentation: `README`, `CONTRIBUTING`, `docs/`, `.cursorrules`, `.claude/`, any style guides.
2. Identify the testing framework: check `package.json`, `pyproject.toml`, `Gemfile`, `go.mod`, or equivalent. Note the test runner command, file naming pattern, and assertion style used in existing tests.
3. Note folder structure, naming conventions, linting/formatting rules.
4. Check `.specs/` for existing bug reports — do not duplicate a report that already exists.

Then read all available evidence from the intake:

- Stack traces and error messages (exact text)
- Relevant source files and config files
- Logs and environment variables provided
- **Absences matter too**: note missing logs, silent failures, or unreachable code paths

Record observations only. No theories yet.

## Phase 3 — Hypothesize

Produce a ranked list of **3–5 root cause candidates**. For each:

| Field | Content |
|-------|---------|
| **Claim** | What is broken and why |
| **Evidence** | Specific observations from Phase 2 that support it (file, line, log line, error text) |
| **Confidence** | High / Medium / Low |

Rank from highest to lowest confidence.

**Gate:** If all hypotheses are Low confidence, ask one targeted clarifying question before continuing to Phase 4. Ask only one question — do not batch them.

## Phase 4 — Verify

Work through hypotheses from highest to lowest confidence. For each:

1. Define a targeted check that would confirm or refute the hypothesis.
2. Write a minimal test case in the project's established testing framework (see Phase 2 findings). The test must:
   - Follow existing conventions: file location, naming pattern, assertion style, test runner
   - Isolate the single failing behavior — no unrelated setup
   - Be runnable, not pseudocode
   - Assert expected vs. actual behavior explicitly
3. Run the test case.
4. If confirmed: stop. Mark the hypothesis confirmed and proceed to Phase 5.
5. If refuted: mark it refuted and move to the next hypothesis.

**If no hypothesis is confirmed after all checks:** return to Phase 2. Use the new negative evidence to narrow the search. Ask one focused clarifying question if needed.

**Reproduce-first rule:** Never propose a fix before a hypothesis is confirmed by a test case or reproduction. If the issue genuinely cannot be reproduced (e.g., production-only, requires external state), say so explicitly — describe what was attempted and why reproduction failed. You may still propose a fix, but mark confidence as Low and note the reproduction gap in the report.

## Phase 5 — Propose and Save

Only after Phase 4 verification is complete:

1. Derive a kebab-case bug name from the issue (e.g., `auth-token-expiry-not-refreshing`, `null-pointer-on-empty-cart`).
2. Check whether `.specs/bugs/[bug-name].md` already exists. If it does, use `[bug-name]-2.md`.
3. Create `.specs/bugs/` if it does not exist.
4. Write the report using the template below.
5. Tell the user the full file path.

### Report Template

```markdown
# Bug: [bug-name]

**Date:** YYYY-MM-DD
**Status:** open

## Root Cause

Plain-language explanation of what broke and why. Cite specific evidence —
file name, line number, log line, or error message.

## Confidence

[High / Medium / Low] — one sentence justifying the confidence level.

## Reproduction

[Minimal test case written in the project's test framework. If reproduction
failed, explain what was attempted and why.]

## Fix

Numbered, concrete steps to resolve the issue. Each step must be actionable
and consistent with the project's conventions and style. No vague instructions
like "handle the error appropriately."

1. ...
2. ...

## Prevention

What would stop this from recurring: a test to add, a monitoring alert, a
config guard, a refactor, or a documentation update. Align with tools and
patterns already in use.

Required for regressions. Encouraged otherwise.
```

## Gotchas

- **Never propose a fix before verifying the hypothesis.** Verification means a test case ran and confirmed the behavior, not that the hypothesis "seems right."
- **Never skip the reproduce step because the issue seems obvious.** Obvious causes are often wrong.
- **Never treat a Low-confidence hypothesis as confirmed.** If confidence is Low after a check, mark it refuted or inconclusive and move on.
- **Ask only one clarifying question at a time.** Multiple questions at once slow resolution and overwhelm the user.
- **Write test cases that match the project's existing conventions.** Do not invent a new testing style. Read existing tests in Phase 2 before writing anything.
- **Never save the report before Phase 4 verification is complete.** The report is an artifact of confirmed diagnosis, not speculation.
- **Always check for an existing `.specs/bugs/` file** before writing. Use the `-2` suffix to avoid overwriting.
- **Absences are evidence.** A missing log line, a silent failure, or an unreachable branch can be as diagnostic as an error message.
