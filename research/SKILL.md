---
name: research
description: >
  Deeply explores a problem space before any spec or design work begins. Use
  when asked to research a topic, explore or understand a problem space, or
  gather context before writing requirements or designing a solution.
---

# Research

Deeply understand a problem, its context, and its constraints before thinking about solutions. The output is a structured `RESEARCH.md` that becomes the primary input for `create-prd` and `create-technical-design`.

**Rule**: Do not propose solutions. Do not sketch architectures. Do not evaluate options. This phase is about understanding — not deciding.

## Routing

- Turn research into requirements → `/create-prd`
- Turn research into architecture → `/create-technical-design`
- Propose and decide between options → `/create-rfc`

## Process

### Step 1 — Intake Interview

**Do not read any code or fetch any URLs yet.**

Use `AskUserQuestion` to gather what the user already knows about the problem space. Capture intent from what they have already shared — only ask for what is genuinely missing. Cover:

- What problem are you trying to solve? Describe it from the perspective of the person experiencing it.
- Who is affected? Be specific about roles, contexts, and frequency.
- What does the current state look like? What workarounds exist today?
- What does a good outcome look like, without prescribing a solution?
- What constraints exist — technical, legal, operational, time, team?
- What is already known or decided? What is still open?
- Are there existing solutions, prior art, or competitor approaches worth studying?
- What would make this research complete? What questions must be answered before spec work can begin?

**Context-gathering rules:**
- Ask one focused topic at a time using `AskUserQuestion`. You may group tightly related sub-questions, but never ask about multiple unrelated topics in a single message.
- When a response introduces a new constraint, stakeholder, edge case, or dependency, follow that branch to resolution before moving on.
- Record anything the user explicitly flags as unknown as an open question — do not invent answers.

### Step 2 — Codebase Scan

Scan the existing codebase to surface technical context relevant to the problem. Focus only on what bears on the problem; do not do a broad survey.

Look for:
- Existing modules, services, or components that relate to the problem domain
- Established patterns: file structure, naming conventions, data flow, testing approach
- Prior decisions recorded in `.specs/` (ADRs, RFCs, PRDs, technical designs) that overlap with this problem
- Technical debt, known limitations, or constraints visible in the code or comments
- Integration boundaries: external services, APIs, or data stores that are in play

Record what you find as observations, not conclusions. Do not infer solutions.

### Step 3 — External Research

Using the references and topics surfaced in Steps 1 and 2, fetch and read relevant external sources. This may include:

- Documentation for technologies, APIs, or standards involved
- Prior art: open-source solutions, published approaches, or industry patterns
- Competitor or analogous product approaches mentioned by the user
- Standards, compliance requirements, or legal references

**Rules:**
- Only fetch URLs the user provided or URLs surfaced from codebase findings. Do not guess or invent URLs.
- Summarize what is relevant. Do not reproduce large blocks of external content verbatim.
- Record the source URL and the key finding from each reference.

### Step 4 — Synthesize and Draft

Compile everything gathered across Steps 1–3 into a `RESEARCH.md` document using the template below.

**File location:** `.specs/[feature-slug]/RESEARCH.md` — derive the slug from the feature or problem name (lowercase, hyphen-separated). Create the directory if it does not exist.

Before drafting, verify:
- Every section can be written from observed facts — not invented conclusions
- Open questions are recorded honestly, not papered over
- No solution or architecture is proposed anywhere in the document

### Step 5 — Recommend Next Steps

After saving the document, present the user with the recommended next step based on what the research revealed:

```
Research complete: .specs/[feature-slug]/RESEARCH.md

Suggested next steps:
- Run /create-prd to turn this research into a product requirements document
- Run /create-technical-design if the problem is primarily technical and requirements are clear
- Run /create-rfc if a significant decision needs alignment before moving forward
```

## Template

```markdown
# Research — [Feature or Problem Name]

| Field   | Value       |
|---------|-------------|
| Author  | @Name       |
| Date    | YYYY-MM-DD  |
| Status  | Draft / Final |

## Problem Framing

What is the problem, stated from the perspective of the person experiencing it? Describe the gap between the current state and the desired state. Do not propose a solution.

**Current state**: [What exists today and how it falls short]

**Desired state**: [What a good outcome looks like, without prescribing a solution]

**Impact**: [Who is affected, how frequently, and what the cost of the status quo is]

## Stakeholders & Users

| Stakeholder / User | Role or Context | Primary Need or Pain |
|--------------------|-----------------|----------------------|
| [Name or type] | [When/where they encounter this] | [What they need or what hurts] |

Add one row per distinct stakeholder or user type.

## Constraints

Hard limits that any solution must respect. Non-negotiable.

### Technical
- [Constraint]: [explanation]

### Legal / Compliance
- [Constraint]: [explanation]

### Operational
- [Constraint]: [explanation]

### Time / Team
- [Constraint]: [explanation]

Write "None identified." for any category with no constraints.

## Existing Solutions & Prior Art

What has already been tried, built, or considered? What can be learned from analogous solutions?

| Solution / Approach | Source | Key Finding | Applicability |
|---------------------|--------|-------------|---------------|
| [Name or description] | [Internal / URL] | [What it does or reveals] | [High / Medium / Low — and why] |

Write "None identified." if no relevant prior art was found.

## Codebase Findings

Observations from scanning the existing codebase that are relevant to this problem. Observations only — no conclusions about what to build.

- **[Module / file / pattern]**: [What it does and why it is relevant]

Write "Not applicable." if there is no existing codebase relevant to this problem.

## External References

Sources consulted during research.

| Reference | URL | Key Finding |
|-----------|-----|-------------|
| [Title or description] | [URL] | [What it contributes to the understanding of this problem] |

Write "None." if no external sources were consulted.

## Open Questions

Questions that must be answered before or during spec and design work. These are known unknowns — record them honestly.

| Question | Why It Matters | Owner | Status |
|----------|---------------|-------|--------|
| [Question] | [Impact if unresolved] | [Person or team] | Open / In Progress / Resolved |

Write "None." if all questions have been resolved.

## Recommended Next Steps

What should happen after this research is complete?

- [Step 1 with skill reference if applicable]
- [Step 2]
```

## Validation Checklist

- [ ] Problem Framing states the problem from the user's perspective — no solutions proposed
- [ ] Desired state describes an outcome, not an implementation
- [ ] Stakeholders & Users has one row per distinct persona
- [ ] All four Constraints categories are addressed (or explicitly marked "None identified.")
- [ ] Existing Solutions includes internal and external prior art
- [ ] Codebase Findings lists specific modules or patterns, not vague generalities
- [ ] External References includes source URL and key finding for each entry
- [ ] Open Questions records every unresolved question honestly
- [ ] No architecture, technology choice, or solution is proposed anywhere in the document
