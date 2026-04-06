---
name: implement
description: Executes implementation tasks by consuming existing PRD and TDD artifacts. Use when the user says "implement this", "implement phase N", "build this feature", "start implementing", "execute the plan", or wants to turn requirements and design documents into working code using TDD (Red-Green-Refactor). Do NOT use for creating PRDs, TDDs, RFCs, or ADRs.
---

# Implementation Executor

You execute implementation tasks by translating existing requirements (PRD) and technical design (TDD) into working, tested code. You follow strict Test-Driven Development and deliver work in small, incremental slices.

## Implement vs Create-PRD

| Aspect | Implement | Create-PRD |
|--------|-----------|------------|
| **Purpose** | Turn requirements into working code | Define what to build and why |
| **Input** | PRD + TDD artifacts | User's problem space |
| **Output** | Tested, working code | Product requirements document |
| **Timing** | After PRD and TDD exist | Before implementation planning |

Use Implement when requirements and design are defined and you need to **write the code**. Use Create-PRD when you need to **define what to build**.

## Implement vs Create-TDD

| Aspect | Implement | Create-TDD |
|--------|-----------|------------|
| **Purpose** | Turn requirements into working code | Design the technical approach |
| **Input** | PRD + TDD artifacts | Requirements + problem space |
| **Output** | Tested, working code | Architecture + implementation plan |
| **Timing** | After technical design is complete | After requirements, before coding |

Use Implement when the architecture is decided and you need to **write the code**. Use Create-TDD when you need to **design the technical approach**.

## Core Principle: Small Vertical Slices with TDD

Every unit of work follows **Red-Green-Refactor**:

1. **Red** — Write one failing test for the smallest meaningful behavior
2. **Green** — Write the minimum code to make the test pass
3. **Refactor** — Improve code structure while keeping all tests green

Each slice delivers a thin but complete piece of functionality that can be verified independently. Never implement multiple behaviors before writing tests. Never write tests after the implementation.

## Process

### Step 1 — Understand the Context

Locate and read the relevant artifacts:

- Look for PRD at `.specs/[feature-slug]/PRD.md`
- Look for TDD at `.specs/[feature-slug]/TDD.md`

If the user specifies a feature slug or path, use that. If not, ask using AskUserQuestion.

If either artifact is missing, do not proceed. Instead, tell the user which artifact is missing and route them to the appropriate skill:

- Missing PRD: "No PRD found. Use `/create-prd` to define the requirements first."
- Missing TDD: "No TDD found. Use `/create-tdd` to define the technical design first."
- Missing both: "No PRD or TDD found. Start with `/create-prd` to define requirements, then `/create-tdd` for the technical design."

After reading the artifacts, extract:

- **Scope**: What requirements (from PRD) are in play for this task
- **Architecture**: What technical approach (from TDD) applies
- **Constraints**: Hard limits from both documents
- **Acceptance criteria**: How to verify the work is correct
- **Phase**: If the TDD defines implementation phases, identify which phase to implement

If the task scope is ambiguous (e.g., "implement the feature" when the TDD has 4 phases), ask the user to clarify which phase or subset of requirements to tackle.

Present a brief summary of your understanding to the user before proceeding:

```
Implementing: [task description]
Phase: [N, if applicable]
Requirements: [list of requirement IDs from PRD]
Approach: [1-2 sentences from TDD]
Constraints: [key constraints]
```

Wait for user confirmation before moving to Step 2.

### Step 2 — Break the Phase into Tasks

Each TDD phase is already a vertical slice. Do not re-slice it. Instead, if the phase contains multiple distinct behaviors, break it into ordered tasks. Each task should be completable in a single TDD cycle (Red-Green-Refactor).

If the phase is small enough for a single TDD cycle, skip the breakdown and move directly to Step 3.

When a breakdown is needed, present the task list to the user:

```
Task 1: [behavior description]
Task 2: [behavior description]
Task 3: [behavior description]
...
```

Wait for user confirmation before starting execution.

### Step 3 — Execute (Red-Green-Refactor)

For each task, follow this cycle strictly:

#### 3a. Red — Write a Failing Test

- Write one test that captures the expected behavior for this task
- Run the test suite and confirm the new test fails
- If the test passes immediately, the behavior already exists — skip to the next task

**Important**: Run the tests. Do not assume the result. The Red step is only complete when you have observed the failure.

#### 3b. Green — Minimal Implementation

- Write the minimum code necessary to make the failing test pass
- Run the full test suite (not just the new test)
- If any test fails, fix the implementation before proceeding
- Do not add code beyond what the test requires

#### 3c. Refactor

- With all tests green, look for improvements:
  - Remove duplication introduced by the new code
  - Improve naming if the intent is unclear
  - Simplify structure if complexity crept in
- Run the full test suite after each refactor change
- If any test breaks during refactor, revert the refactor change and try a different approach

#### 3d. Move to Next Task

After completing one task, briefly state what was done and move to the next. Do not wait for user input between tasks unless something unexpected arises (ambiguity, a design decision not covered by the TDD, a conflict with existing code).

**When to pause and ask**:
- A requirement is ambiguous and multiple interpretations are valid
- The TDD's design does not account for a discovered constraint
- Existing code conflicts with the planned approach
- A dependency is missing or unavailable

### Step 4 — Review and Validate

After all tasks are complete:

1. **Run the full test suite** — all tests must pass
2. **Run linters and formatters** — use whatever is configured in the project (Prettier, ESLint, Biome, etc.). Fix any issues.
3. **Verify against PRD** — check each requirement that was in scope. Confirm the implementation satisfies it.
4. **Verify against TDD** — confirm the implementation follows the architectural decisions and contracts defined in the TDD.

If any verification fails, return to Step 3 and address the gap.

### Step 5 — Summarize and Request Feedback

Briefly summarize what was implemented, any decisions made that were not covered by the artifacts, and open questions if any. Keep it short — the code and tests speak for themselves.

Wait for user feedback. If the user requests changes, return to Step 3 for the affected tasks.

### Step 6 — Iterate

When the user provides feedback:

1. Identify which tasks are affected
2. Update or add tests to reflect the new expectations (Red)
3. Update the implementation (Green)
4. Refactor if needed
5. Run the full validation cycle (Step 4)
6. Present an updated summary (Step 5)

Repeat until the user is satisfied.

### Step 7 — Commit (Manual Gate)

**Never commit automatically.** When the user is satisfied with the implementation:

1. Propose a commit message following the project's conventions:

```
[type]: [concise description]

[Body explaining what changed and why, referencing requirement IDs if applicable]
```

2. List the files that will be staged
3. Wait for explicit user confirmation before committing

If the user asks for changes to the commit message, adjust and re-present. Only commit when the user explicitly approves.

## When to Stop and Ask

Pause execution and ask the user when:

- **Ambiguous scope**: The task could reasonably be interpreted multiple ways
- **Design gap**: The TDD does not cover a situation encountered during implementation
- **Conflicting requirements**: Two requirements contradict each other
- **Missing dependency**: A package, service, or tool needed for implementation is not available
- **Significant deviation**: The implementation needs to differ materially from the TDD's design

Do not invent answers to these situations. Surface them and let the user decide.

## Common Mistakes

**Implementing without reading the artifacts first**
The PRD and TDD exist for a reason. Read them fully before writing any code. Do not rely on the user's verbal summary alone.

**Writing tests after the implementation**
This is not TDD. Always write the test first, confirm it fails, then implement. Tests written after the fact tend to test the implementation rather than the behavior.

**Bundling multiple behaviors into one task**
Wrong: "Implement user registration with validation, email confirmation, and profile creation."
Right: Three separate tasks — one for registration, one for validation, one for email confirmation. Profile creation might be a separate phase entirely.

**Refactoring while tests are red**
Never refactor when a test is failing. Get to Green first, then refactor. Refactoring on Red makes it impossible to know whether the refactor broke something or the implementation was already wrong.

**Committing without explicit approval**
This skill never commits automatically. Always present the proposed commit and wait.

**Over-implementing beyond what the test demands**
In the Green step, write only the code the failing test requires. Anticipatory code without a corresponding test leads to untested paths and unnecessary complexity.

## Example Prompts

- "Implement phase 1 of the user onboarding feature"
- "Implement the payment processing flow from the TDD"
- "Start implementing the auth module"
- "Build the API endpoints defined in the TDD"
- "Execute the implementation plan for stripe-integration"
