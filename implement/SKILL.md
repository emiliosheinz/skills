---
name: implement
description: Executes implementation tasks by consuming existing PRD, technical design, and implementation plan artifacts. Use when the user says "implement this", "implement phase N", "build this feature", "start implementing", "execute the plan", or wants to turn requirements and design documents into working code using TDD (Red-Green-Refactor). Do NOT use for creating PRDs, technical designs, implementation plans, RFCs, or ADRs.
---

# Implementation Executor

You execute implementation tasks by translating existing requirements (PRD), technical design, and implementation plan into working, tested code. You follow strict Test-Driven Development and deliver work in small, incremental slices.

## Skill Routing

| Signal | Skill |
|--------|-------|
| Requirements and design exist, need working code | **Implement** (this skill) |
| Need to define what to build and why | `/create-prd` |
| Need to design the technical approach | `/create-technical-design` |
| Need to break work into phases and tasks | `/create-implementation-plan` |

## Core Principle: Small Vertical Slices with TDD

Every unit of work follows **Red-Green-Refactor**:

1. **Red** -- Write one failing test for the smallest meaningful behavior
2. **Green** -- Write the minimum code to make the test pass
3. **Refactor** -- Improve code structure while keeping all tests green

Each slice delivers a thin but complete piece of functionality that can be verified independently. Never implement multiple behaviors before writing tests. Never write tests after the implementation.

## Staying Lean

Implementation generates a lot of context: artifact contents, test output, code diffs, lint results. Be intentional about what you hold onto.

- **Read selectively.** You rarely need the full PRD and technical design at once. Start with the implementation plan to identify the target phase, then pull in only the requirements and design sections that phase touches.
- **Extract, then let go.** Once you have pulled out the facts you need from an artifact (requirement IDs, API contracts, constraints), you do not need the raw document anymore. Re-read a specific section later if something comes up.
- **Shed finished work.** After a task's TDD cycle is green, the test output and code diff have served their purpose. Move on. Re-read a file only when the next task needs to reference it.
- **Delegate heavy lifting.** When a chunk of work is self-contained (running the full test suite, verifying requirements against the PRD, a single Red-Green-Refactor cycle), consider handing it to a sub-agent so the output stays out of your context. Give sub-agents specific inputs: file paths, section references, constraints -- not open-ended instructions.

These are guidelines, not rules. Use your judgment about what to hold, what to shed, and when delegation is worth the overhead.

## Process

### Step 1 -- Understand the Context

Locate and read the relevant artifacts:

- Look for PRD at `.specs/[feature-slug]/PRD.md`
- Look for technical design at `.specs/[feature-slug]/TECHNICAL-DESIGN.md`
- Look for implementation plan at `.specs/[feature-slug]/IMPLEMENTATION-PLAN.md`

If the user specifies a feature slug or path, use that. If not, ask using AskUserQuestion.

If required artifacts are missing, do not proceed. Instead, tell the user which artifact is missing and route them to the appropriate skill:

- Missing PRD: "No PRD found. Use `/create-prd` to define the requirements first."
- Missing technical design: "No technical design found. Use `/create-technical-design` to define the architecture first."
- Missing implementation plan: "No implementation plan found. Use `/create-implementation-plan` to define phases and tasks first."
- Missing all: "No artifacts found. Start with `/create-prd`, then `/create-technical-design`, then `/create-implementation-plan`."

After reading the artifacts, extract:

- **Scope**: What requirements (from PRD) are in play for this task
- **Architecture**: What technical approach (from technical design) applies
- **Constraints**: Hard limits from the artifacts
- **Acceptance criteria**: How to verify the work is correct
- **Phase**: Which phase from the implementation plan to implement

If the task scope is ambiguous (e.g., "implement the feature" when the implementation plan has 4 phases), ask the user to clarify which phase or subset of requirements to tackle.

Present a brief summary of your understanding to the user before proceeding:

```
Implementing: [task description]
Phase: [N, if applicable]
Requirements: [list of requirement IDs from PRD]
Approach: [1-2 sentences from technical design]
Constraints: [key constraints]
```

Wait for user confirmation before moving to Step 2.

### Step 2 -- Break the Phase into Tasks

Each implementation plan phase is already a vertical slice. Do not re-slice it. Instead, if the phase contains multiple distinct behaviors, break it into ordered tasks. Each task should be completable in a single TDD cycle (Red-Green-Refactor).

If the phase is small enough for a single TDD cycle, skip the breakdown and move directly to Step 3.

When a breakdown is needed, present the task list to the user:

```
Task 1: [behavior description]
Task 2: [behavior description]
Task 3: [behavior description]
...
```

Wait for user confirmation before starting execution.

### Step 3 -- Execute (Red-Green-Refactor)

For each task, follow this cycle strictly:

#### 3a. Red -- Write a Failing Test

- Write one test that captures the expected behavior for this task
- Run the test suite and confirm the new test fails
- If the test passes immediately, the behavior already exists -- skip to the next task

**Important**: Run the tests. Do not assume the result. The Red step is only complete when you have observed the failure.

#### 3b. Green -- Minimal Implementation

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

After completing one task, briefly state what was done and move to the next. Do not wait for user input between tasks unless something unexpected arises (ambiguity, a design decision not covered by the technical design, a conflict with existing code).

**When to pause and ask**:
- A requirement is ambiguous and multiple interpretations are valid
- The technical design does not account for a discovered constraint
- Existing code conflicts with the planned approach
- A dependency is missing or unavailable

### Step 4 -- Review and Validate

After all tasks are complete:

1. **Run the full test suite** -- all tests must pass
2. **Run linters and formatters** -- use whatever is configured in the project (Prettier, ESLint, Biome, etc.). Fix any issues.
3. **Verify against PRD** -- check each requirement that was in scope. Confirm the implementation satisfies it.
4. **Verify against technical design** -- confirm the implementation follows the architectural decisions and contracts defined in the technical design.

These checks are independent -- run them in parallel when possible.

If any verification fails, return to Step 3 and address the gap.

### Step 5 -- Summarize and Request Feedback

Briefly summarize what was implemented, any decisions made that were not covered by the artifacts, and open questions if any. Keep it short -- the code and tests speak for themselves.

Wait for user feedback. If the user requests changes, return to Step 3 for the affected tasks.

### Step 6 -- Iterate

When the user provides feedback:

1. Identify which tasks are affected
2. Update or add tests to reflect the new expectations (Red)
3. Update the implementation (Green)
4. Refactor if needed
5. Run the full validation cycle (Step 4)
6. Present an updated summary (Step 5)

Repeat until the user is satisfied.

### Step 7 -- Commit (Manual Gate)

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
- **Design gap**: The technical design does not cover a situation encountered during implementation
- **Conflicting requirements**: Two requirements contradict each other
- **Missing dependency**: A package, service, or tool needed for implementation is not available
- **Significant deviation**: The implementation needs to differ materially from the technical design

Do not invent answers to these situations. Surface them and let the user decide.

## Common Mistakes

**Implementing without reading the artifacts first.**
The PRD, technical design, and implementation plan exist for a reason. Read them before writing any code. Do not rely on the user's verbal summary alone.

**Writing tests after the implementation.**
This is not TDD. Always write the test first, confirm it fails, then implement. Tests written after the fact tend to test the implementation rather than the behavior.

**Bundling multiple behaviors into one task.**
Wrong: "Implement user registration with validation, email confirmation, and profile creation."
Right: Three separate tasks -- one for registration, one for validation, one for email confirmation.

**Refactoring while tests are red.**
Never refactor when a test is failing. Get to Green first, then refactor.

**Committing without explicit approval.**
This skill never commits automatically. Always present the proposed commit and wait.

**Over-implementing beyond what the test demands.**
In the Green step, write only the code the failing test requires. Anticipatory code without a corresponding test leads to untested paths and unnecessary complexity.
