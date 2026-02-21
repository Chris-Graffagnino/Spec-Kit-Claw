# The Art of Building Technical Specifications: A Decision Framework

**A comprehensive guide to Spec-Driven Development — from initial idea to executable plan, inspired by the philosophy and methodology behind GitHub's Spec Kit.**

---

## License
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Why Specifications Matter

For decades, software development has treated code as the primary artifact and specifications as disposable scaffolding — documents written once, skimmed by a few, and abandoned the moment implementation begins. The result is a persistent gap between what teams *intended* to build and what they actually ship. Requirements drift. Assumptions diverge. And the further you get from the original idea, the harder it is to correct course.

Spec-Driven Development (SDD) inverts this relationship. Specifications are not guides for implementation — they are the **source of truth** that *generates* implementation. The code serves the spec, not the other way around. When something changes, you update the spec and regenerate downstream artifacts, rather than patching symptoms across a scattered codebase.

This document lays out the planning process and decision framework for creating technical specifications using a spec-driven approach. It is not about code. It is about the thinking, the structure, and the discipline that precedes it.

---

## The Core Principles

Before diving into phases and templates, it's worth internalizing the philosophical commitments that make spec-driven work different from traditional requirements gathering.

### Intent Before Implementation

Every specification begins with the *what* and *why*, never the *how*. Technology choices, frameworks, and architecture come later. The first job is to articulate what the software should accomplish for its users and why that outcome matters. This sounds obvious, but it is routinely violated in practice when teams lead with a stack decision or a solution architecture before they've clearly stated the problem.

### Specifications as Living Artifacts

A specification is not a snapshot in time. It is a living document that evolves with the project. When you need a new feature, you revisit the specification. When you want to explore an alternative implementation, you branch the specification. The spec is version-controlled alongside the code, and its change history tells the story of the project's evolution.

### Multi-Step Refinement Over One-Shot Generation

Good specifications are not written in a single pass. They emerge through iterative dialogue — between team members, between humans and AI, between product vision and technical reality. Each refinement cycle catches ambiguities, surfaces edge cases, and tightens acceptance criteria.

### Guardrails and Governance

Specifications don't exist in a vacuum. They operate within organizational constraints — approved technology stacks, compliance requirements, design systems, performance budgets, and engineering conventions. These constraints are codified explicitly so they can be referenced and enforced systematically, not rediscovered through trial and error on each new project.

---

## The Development Phases

The spec-driven process follows a strict, gated sequence. Each phase produces or updates specific artifacts, and you don't advance to the next phase until the current one is validated. The phases are:

1. **Constitution** — Establish governing principles
2. **Specify** — Define what to build and why
3. **Plan** — Translate intent into a technical approach
4. **Tasks** — Decompose the plan into executable units of work
5. **Analyze** — Validate coherence before committing to build
6. **Implement** — Execute against the task list

The following sections walk through each phase in detail, including the key decisions, the questions you should be asking, and the artifacts you should be producing.

---

## Phase 1: Constitution — Establishing Your Governing Principles

### What It Is

The constitution is a set of **non-negotiable principles** that govern how every specification in your project (or organization) is written, planned, and implemented. Think of it as the organizational DNA — the rules that don't change from feature to feature.

### Why It Comes First

Without a constitution, every new specification reinvents its own ground rules. One feature might assume a microservices architecture while another assumes a monolith. One team might require 90% test coverage while another writes no tests at all. The constitution eliminates this inconsistency by defining shared standards before any feature work begins.

### What to Define

A constitution typically addresses the following concerns:

**Architectural Principles.** Decide on your core structural commitments. Should every feature begin as a standalone, reusable library? Should all functionality be accessible through a CLI interface? Should components follow a particular modularity pattern? These are the decisions that, once made, should not be revisited on a per-feature basis.

**Technology Guardrails.** Capture the approved stack, required frameworks, preferred languages, and any explicit exclusions. If your organization mandates a specific cloud provider or prohibits certain dependencies, this is where you document it.

**Quality Standards.** Define what "done" means. This includes testing strategy (unit, integration, contract, end-to-end), code coverage expectations, documentation requirements, and performance budgets.

**Design Philosophy.** Articulate the values that should guide tradeoffs. For example: observability over opacity, simplicity over cleverness, integration testing over mocking, modularity over monoliths. These philosophies shape how ambiguous decisions are resolved downstream.

**Compliance and Security.** If your domain has regulatory requirements — data residency, access control models, audit logging, encryption standards — they belong in the constitution. These constraints must be visible to anyone writing a specification so they are built in from the start, not bolted on after the fact.

**Commit and Workflow Conventions.** Specify how work should be committed, branched, and reviewed. If every completed task requires its own commit with a structured message, say so here. If deviations from the constitution require documented justification and approval, establish that governance process.

### Key Questions to Ask

- What are the constraints that should apply to *every* feature we build, regardless of its scope?
- What technology decisions have already been made at the organizational level?
- What quality thresholds must be met before any work is considered complete?
- What compliance or regulatory obligations shape our technical choices?
- How do we handle deviations from these principles when exceptions arise?

### The Output

A single, authoritative `constitution.md` document. It should be concise, opinionated, and unambiguous. The constitution is the anchor that all subsequent specification work references.

---

## Phase 2: Specify — Defining What to Build

### What It Is

The specification phase captures the full intent of what you are building. This is the Product Requirements Document (PRD) equivalent in spec-driven terms — but unlike a traditional PRD, this document is not destined to gather dust. It will directly drive the technical plan and task breakdown.

### The Starting Point: An Idea

Every specification begins with an idea — often vague and incomplete. The goal of this phase is to transform that rough idea into a comprehensive, precise description of the desired outcome through iterative refinement.

### How to Approach It

**Lead with the user.** Describe who the software is for and what problem it solves for them. Frame everything in terms of user goals, journeys, and scenarios. Resist the urge to describe how the system should work internally — focus on observable behavior.

**Define the boundaries.** What is in scope? What is explicitly *not* in scope? Scope boundaries are as important as scope inclusions. They prevent feature creep and keep the specification focused.

**Articulate scenarios and acceptance criteria.** For each capability, describe the expected behavior in concrete, testable terms. Acceptance criteria should be specific enough that someone unfamiliar with the project could determine whether a given implementation satisfies them.

**Surface edge cases.** The most costly bugs originate from scenarios nobody thought about during specification. Deliberately probe the boundaries: What happens when input is missing? What happens at scale? What happens when the user does something unexpected?

**Identify constraints and dependencies.** Are there external APIs, data sources, or services the feature depends on? Are there timing constraints, sequencing requirements, or compatibility obligations?

### Iterative Dialogue

The specification should not be written in isolation. It emerges through dialogue — asking clarifying questions, challenging assumptions, and progressively narrowing ambiguity. In an AI-assisted workflow, this means prompting the AI to ask questions rather than accepting its first draft. In a team setting, this means cross-functional review involving product, engineering, design, and QA perspectives.

What might take days of meetings and documentation in traditional development can happen in hours of focused specification work — but only if you invest in the dialogue. Skipping this step and accepting vague requirements is exactly the mistake spec-driven development is designed to prevent.

### What to Watch For

**Technical details bleeding into the spec.** If you find yourself specifying UI element sizes, database column types, or API response formats, you've crossed into planning territory. Move those details out and keep the spec focused on the *what*, not the *how*.

**Over-eagerness.** Whether you are working alone or with AI, there is a strong temptation to expand scope, add "nice to have" features, or over-specify non-functional requirements. Edit ruthlessly. A good specification is precise and minimal.

**Self-assessment.** Before finalizing, review the specification against your constitution. Does it align with your architectural principles? Does it respect your technology guardrails? Does it meet your quality standards?

### The Output

A `spec.md` document (or a set of per-feature specification files) that captures goals, user scenarios, acceptance criteria, constraints, and explicit scope boundaries.

---

## Phase 3: Plan — Translating Intent into Architecture

### What It Is

The planning phase translates the *what* of the specification into the *how* of technical implementation. This is where you make architecture choices, select patterns, define data models, design API contracts, and determine project structure.

### The Relationship Between Spec and Plan

The plan serves the specification, not the other way around. Every decision in the plan should trace back to a requirement in the spec. If you find yourself planning work that has no basis in the specification, either the spec is incomplete (go back and update it) or the plan is introducing unnecessary scope.

### What to Define

**Architecture and Project Structure.** Based on the nature of the project, choose the appropriate structural pattern. A single-purpose library has different needs than a web application with separate frontend and backend, which in turn differs from a mobile application with an API layer. The plan should make this structural decision explicit and document the reasoning.

**Technology Stack Choices.** With the constitution's guardrails as context, select the specific libraries, frameworks, and tools for this feature. Document not just *what* you chose but *why* — what tradeoffs did you evaluate, and what did you prioritize?

**Data Model Design.** Define the entities, their relationships, their lifecycle, and their validation rules. This may result in a dedicated `data-model.md` artifact that captures schema definitions, migrations, and data flow.

**API and Interface Contracts.** If the feature involves APIs, services, or inter-module communication, define the contracts explicitly. What endpoints exist? What are the request and response shapes? What are the error conditions? These contracts may be captured in a dedicated `contracts/` directory.

**Research and Unknowns.** If the plan depends on technologies or integrations that are new or rapidly evolving, identify them explicitly. Create a `research.md` artifact to document open questions, prototype findings, and version-specific details that need verification before implementation begins.

**Quickstart and Validation Scenarios.** Define the key scenarios that will prove the plan works. A quickstart guide captures the critical path — the minimum sequence of actions that exercises the core behavior and demonstrates that the implementation is sound.

### Key Questions to Ask

- Does this architecture naturally support the scenarios described in the specification?
- Are we introducing complexity that isn't justified by the requirements?
- Have we respected the constitutional principles in our technology and design choices?
- What are the riskiest assumptions in this plan, and how will we validate them?
- Are there dependencies between components that constrain the order of implementation?

### Constitutional Compliance

The plan is explicitly checked against the constitution. If the constitution requires that every feature be a standalone library, the plan must reflect that. If the constitution mandates CLI-first interfaces, the plan must include CLI design. This compliance check is not optional — it is a formal gate.

### The Output

A `plan.md` document along with supporting artifacts: `data-model.md`, `research.md`, `quickstart.md`, and a `contracts/` directory as needed. Together, these artifacts form a complete technical blueprint that respects both the specification's intent and the constitution's constraints.

---

## Phase 4: Tasks — Decomposing the Plan into Executable Work

### What It Is

The task phase breaks the implementation plan into small, self-contained, ordered units of work. Each task is specific enough to be completed and validated independently.

### Why Granularity Matters

Large, vaguely defined tasks are the enemy of predictable execution. A task like "build authentication" is too broad — it hides dozens of decisions, dependencies, and potential failure points. A task like "create a user registration endpoint that validates email format and returns structured error responses" is specific, testable, and completable in a single focused session.

The smaller and more precise the task, the easier it is to verify correctness, the faster you can catch mistakes, and the more predictable the overall progress.

### How to Break Down Tasks

**Derive from contracts and entities.** The data model and API contracts defined in the planning phase are natural sources of tasks. Each entity needs creation, each endpoint needs implementation, each contract needs test coverage.

**Respect dependency order.** Tasks should be ordered so that prerequisites come first: data models before services, services before endpoints, endpoints before UI integration. This sequencing prevents blocked work and enables incremental validation.

**Mark parallelizable work.** Independent tasks that have no mutual dependencies can be executed in parallel. Identifying these opportunities and marking them explicitly (often with a `[P]` notation) allows the team — or the AI — to optimize throughput.

**Group by user story or feature slice.** Organize tasks around coherent vertical slices of functionality rather than horizontal technical layers. This ensures that each group of tasks, when completed, delivers visible, testable value.

### What a Good Task Looks Like

Each task should include:

- A clear, specific description of what to build or change
- The files or modules it affects
- Its dependencies on other tasks
- Its acceptance criteria (how to verify it is done)
- Any relevant context from the spec or plan that the implementer needs

### Key Questions to Ask

- Can each task be completed and verified independently?
- Does the ordering respect all dependencies?
- Are there opportunities for parallel execution?
- Does the sum of all tasks account for everything in the plan?
- Are there any implicit tasks (setup, configuration, testing infrastructure) that are missing?

### The Output

A `tasks.md` document (or individual task files in a `tasks/` directory) with an ordered, dependency-aware list of discrete work items, each with enough context for implementation.

---

## Phase 5: Analyze — Validating Coherence Before Building

### What It Is

The analysis phase is a quality gate. Before committing to implementation, you systematically review all artifacts — constitution, specification, plan, and tasks — to ensure they are internally consistent, mutually aligned, and free of contradictions.

### Why This Phase Exists

It is remarkably easy for inconsistencies to accumulate across documents. The specification might describe a feature that the plan doesn't fully account for. The plan might introduce a dependency that contradicts the constitution. A task might reference a contract that was later revised. The analysis phase catches these misalignments before they become expensive implementation bugs.

### What to Check

**Spec-to-plan alignment.** Does every requirement in the specification have a corresponding element in the plan? Does the plan introduce anything that isn't grounded in the specification?

**Plan-to-task coverage.** Does every component, endpoint, and entity in the plan have tasks that produce it? Are there tasks that don't trace back to the plan?

**Constitutional compliance.** Do all artifacts adhere to the governing principles? Are there any violations of the technology guardrails, quality standards, or architectural commitments?

**Internal consistency.** Do the data model, contracts, and quickstart scenarios tell a coherent story? Are naming conventions consistent? Do assumptions align across documents?

**Conflict detection.** Are there any requirements specified in multiple places with inconsistent wording? Over-eager specification can produce redundant or contradictory definitions that need reconciliation.

### Resolving Issues

When the analysis surfaces problems, address them by updating the appropriate upstream artifact. If the spec is ambiguous, clarify the spec. If the plan missed a requirement, update the plan. If tasks are misordered, fix the task list. Never patch downstream — always fix at the source.

### The Output

A validated set of artifacts, with all identified issues resolved. This is the "green light" to proceed to implementation.

---

## Phase 6: Implement — Executing Against the Plan

### What It Is

Implementation is the execution phase where the validated task list is worked through systematically. Each task is completed, verified against its acceptance criteria, and committed according to the conventions established in the constitution.

### The Role of the Specification During Implementation

The specification doesn't become irrelevant once implementation starts. It remains the authoritative reference for *what* the software should do. When an implementer (human or AI) encounters an ambiguous situation, the answer should be in the spec. If it isn't, that's a signal to pause and update the spec before proceeding.

### Incremental Validation

Each task should be validated upon completion — not just syntactically (does it compile?) but behaviorally (does it satisfy the acceptance criteria?). This incremental approach catches errors early, when they're cheapest to fix, and ensures that the implementation stays aligned with the specification throughout.

### When to Loop Back

Implementation will inevitably surface issues that weren't anticipated in earlier phases. A data model might prove insufficient. A contract might need revision. A requirement might turn out to be infeasible. When this happens, the right response is to update the upstream artifact (spec, plan, or tasks) and propagate the change — not to silently deviate from the plan.

---

## The Iterative Nature of the Process

Spec-driven development is not a waterfall methodology with a new name. The phases are sequential for any given cycle, but the overall process is deeply iterative. The workflow is best understood as:

**0 → 1 → (1', 1'', ...) → 2 → 3 → N**

The first specification takes you from zero to one. Subsequent iterations refine, extend, or reimagine that foundation. Adding a feature means revisiting the specification and generating a new plan. Exploring an alternative architecture means branching the spec and creating a parallel implementation plan. The specification is always the starting point.

This iterative model transforms the traditional software development lifecycle. Requirements and design are no longer discrete phases that happen once at the beginning — they become continuous activities woven throughout the project's life.

---

## Decision Framework: When to Use This Approach

Spec-driven development adds structure and discipline. That structure has a cost — in time, in token usage (for AI-assisted workflows), and in cognitive overhead. It isn't always the right tool. Here's a framework for deciding.

### Strong Fit

- **Team projects** where multiple people need to work from a shared understanding
- **Complex features** with many interdependent components and non-obvious edge cases
- **Compliance-sensitive domains** where requirements traceability is mandatory
- **AI-assisted development** where you need to constrain and guide AI behavior precisely
- **Long-lived projects** where the specification will be revisited and evolved over months or years
- **Mission-critical applications** where the cost of misalignment between intent and implementation is high

### Weaker Fit

- **Quick prototypes** where the goal is rapid exploration and the output is disposable
- **Trivial changes** where the overhead of a full specification cycle exceeds the complexity of the work
- **Highly uncertain domains** where you don't yet know enough to write a meaningful specification (though even here, a lightweight specification of what you're trying to learn can be valuable)

### The Middle Ground

For simpler work, you don't need the full ceremony. A lightweight plan and a focused task list — without the full constitution, formal analysis, and multi-document artifact suite — can capture most of the value at a fraction of the cost. The key insight is not the specific artifacts but the *discipline* of thinking before coding, defining before building, and validating before shipping.

---

## Common Pitfalls and How to Avoid Them

**Writing specs that are really plans.** If your specification mentions database schemas, API frameworks, or CSS class names, it has crossed the line into planning. Keep the spec focused on observable behavior and user outcomes.

**Skipping the constitution.** Without shared principles, each specification reinvents its own ground rules. This creates inconsistency across features and makes it impossible to validate compliance systematically.

**Accepting the first draft.** The most common failure mode is insufficient iteration. The first version of any specification is incomplete. Invest in the dialogue — ask questions, challenge assumptions, probe edge cases.

**Over-specifying.** More detail is not always better. Excessively granular specifications are brittle and expensive to maintain. Specify at the level of observable behavior, not implementation mechanics.

**Failing to loop back.** When implementation reveals that the spec was wrong, the temptation is to fix the code and move on. Resist this. Update the spec, update the plan, and keep the artifacts aligned. The specification only works as a source of truth if you actually maintain it as one.

**Treating specs as permanent.** Specifications are living documents, not contracts set in stone. They should evolve as understanding deepens, requirements change, and the market shifts. The goal is alignment, not rigidity.

---

## Summary: The Spec-Driven Mindset

Spec-driven development is, at its core, a bet on a simple idea: **the hardest part of software development is not writing code — it is knowing what to build.** Code can be generated, refactored, and replaced. But getting the requirements right, aligning stakeholders, and maintaining intent through months of iteration — that's where the real value lies.

The process laid out in this document — constitution, specify, plan, tasks, analyze, implement — is a structured way to do that hard work *before* the code flows. It doesn't eliminate uncertainty, and it doesn't guarantee success. But it dramatically reduces the gap between what you intended and what you ship, and it creates a clear, version-controlled trail from idea to implementation that any team member can follow.

The specification is the source of truth. Everything else flows from it.

---

## Appendix: Artifact Templates

The templates below use a fictional **bookmark manager CLI tool** as a running example. Each template is minimal but complete — it shows every required section with short placeholders. Replace `[bracketed instructions]` with your project's specifics.

---

### Template: `constitution.md`

```markdown
# Constitution: [Project Name]

## Architectural Principles
- Every feature is a standalone module with a public API and no side effects on import.
- CLI-first: all functionality must be accessible via command-line interface before any GUI is considered.
- [Add principles that apply to every feature, not just one]

## Technology Guardrails
- Language: [e.g., TypeScript 5.x, Node 20+]
- Prohibited: [e.g., no ORM — use raw SQL with parameterized queries]
- Required: [e.g., all HTTP via `fetch`; no third-party HTTP client libraries]

## Quality Standards
- 80% line coverage minimum; 100% coverage on public API surface.
- Every public function has a JSDoc summary and at least one usage example.
- [Add testing strategy: unit, integration, e2e expectations]

## Design Philosophy
- Observability over opacity — prefer structured logs and explicit error codes.
- Simplicity over cleverness — no metaprogramming or dynamic dispatch.
- [Add 1-2 more value statements that guide ambiguous tradeoffs]

## Compliance & Security
- All user data encrypted at rest using AES-256.
- No secrets in source — use environment variables exclusively.
- [Add regulatory or domain-specific requirements]

## Workflow Conventions
- Each completed task = one atomic commit with message format: `type(scope): description`.
- Deviations from this constitution require a written justification in the PR description.
- [Add branching, review, or CI/CD conventions]
```

---

### Template: `spec.md`

```markdown
# Spec: [Feature Name]

## Overview & Problem Statement
[1-2 sentences describing the user problem this feature solves.]

Example: Users have no way to save, tag, or retrieve URLs from the command line, forcing them to rely on browser-based bookmark managers that don't integrate with developer workflows.

## User Stories & Scenarios
1. **Add a bookmark.** As a user, I run `bm add <url> --tags=<t1,t2>` and the bookmark is persisted with the given tags.
2. **Search bookmarks.** As a user, I run `bm search <query>` and see matching bookmarks ranked by relevance.
3. [Add additional stories — one per distinct capability]

## Acceptance Criteria
- `bm add https://example.com --tags=dev,ref` stores the bookmark and prints a confirmation with the assigned ID.
- `bm search "example"` returns all bookmarks whose URL or tags contain "example", sorted by most recent.
- [Add one criterion per story — specific, testable, no implementation details]

## Scope
**In scope:** Add, search, list, delete bookmarks; tag management; local SQLite storage.
**Out of scope:** Sync across devices; browser extension; GUI; user accounts.

## Constraints & Dependencies
- Must work offline — no network calls except to fetch URL metadata on `bm add`.
- Depends on SQLite via [approved library from constitution].
- [Add external APIs, timing constraints, compatibility requirements]

## Edge Cases
- Adding a duplicate URL: update tags instead of creating a second entry.
- Searching with no results: print a helpful message, exit code 0.
- [Add 2-5 more edge cases discovered during iterative dialogue]
```

---

### Template: `plan.md`

```markdown
# Plan: [Feature Name]

## Architecture Overview
A single CLI entry point (`src/cli.ts`) dispatches to command modules (`src/commands/`). Each command calls a shared storage layer (`src/store.ts`) backed by SQLite. No external services.

[Replace with a 2-4 sentence summary of your architecture. Include a simple diagram if helpful.]

## Technology Choices
| Choice | Selected | Rationale |
|--------|----------|-----------|
| CLI framework | yargs | Constitution requires Node-native; yargs has zero native deps |
| Database | better-sqlite3 | Synchronous API simplifies CLI flow; constitution prohibits ORMs |
| [Add rows for each significant choice] | | |

## Data Model Summary
- **Bookmark**: `id` (INTEGER PK), `url` (TEXT UNIQUE), `title` (TEXT), `created_at` (ISO 8601)
- **Tag**: `id` (INTEGER PK), `name` (TEXT UNIQUE)
- **BookmarkTag**: `bookmark_id` (FK), `tag_id` (FK) — many-to-many join
- [Define each entity, its key fields, and relationships. Reference `data-model.md` for full schema.]

## API / Interface Contracts Summary
- `bm add <url> [--tags=t1,t2] [--title=str]` → stdout: `Added bookmark #<id>: <url>`
- `bm search <query> [--tag=t]` → stdout: one line per result `#<id> <url> [tags]`
- `bm delete <id>` → stdout: `Deleted bookmark #<id>` or stderr: `Not found` (exit 1)
- [List every command/endpoint with input → output contract]

## Project Structure
```
src/
  cli.ts          # Entry point, arg parsing
  commands/
    add.ts        # Add-bookmark command
    search.ts     # Search command
    delete.ts     # Delete command
  store.ts        # SQLite operations
  types.ts        # Shared type definitions
```
[Adapt to your project. Show directories and key files only.]

## Research & Unknowns
- SQLite full-text search (FTS5): need to verify `better-sqlite3` supports FTS5 on all target platforms.
- [List any unresolved technical questions. Reference `research.md` for deep dives.]

## Quickstart Scenario
1. `npm install && npm run build`
2. `bm add https://example.com --tags=demo`
3. `bm search "example"` → expect one result
4. `bm delete 1` → expect confirmation
[The shortest sequence that proves the core architecture works end-to-end.]
```

---

### Template: `tasks.md`

```markdown
# Tasks: [Feature Name]

## Task 1 — Initialize project and configure tooling
- **Files:** `package.json`, `tsconfig.json`, `.eslintrc`
- **Depends on:** None
- **Criteria:** `npm run build` succeeds with zero errors; linter passes.

## Task 2 — Implement SQLite storage layer
- **Files:** `src/store.ts`, `src/types.ts`
- **Depends on:** Task 1
- **Criteria:** Unit tests pass for `createBookmark`, `searchBookmarks`, `deleteBookmark`.

## Task 3 — Implement `add` command [P]
- **Files:** `src/commands/add.ts`, `src/cli.ts`
- **Depends on:** Task 2
- **Criteria:** `bm add https://example.com --tags=dev` stores row and prints confirmation.

## Task 4 — Implement `search` command [P]
- **Files:** `src/commands/search.ts`, `src/cli.ts`
- **Depends on:** Task 2
- **Criteria:** `bm search "example"` returns matching bookmarks; empty query returns all.

## Task 5 — Implement `delete` command [P]
- **Files:** `src/commands/delete.ts`, `src/cli.ts`
- **Depends on:** Task 2
- **Criteria:** `bm delete 1` removes the row; deleting a non-existent ID prints error to stderr, exits 1.

## Task 6 — Integration tests and edge cases
- **Files:** `tests/integration/`
- **Depends on:** Tasks 3, 4, 5
- **Criteria:** All acceptance criteria from `spec.md` are covered; `npm test` passes with ≥80% coverage.

[Tasks marked [P] can be executed in parallel. All others must run in dependency order.]
[Add/remove tasks to match your plan. Each task = one atomic commit.]
```

---

### Template: `analysis.md`

```markdown
# Analysis: [Feature Name]

## Spec → Plan Alignment
- [ ] Every user story in `spec.md` maps to at least one component in `plan.md`.
- [ ] No component in `plan.md` exists without a corresponding spec requirement.
- [ ] All scope exclusions in `spec.md` are respected — plan introduces nothing out-of-scope.

## Plan → Task Coverage
- [ ] Every entity in the data model has tasks that create and test it.
- [ ] Every CLI command / API endpoint has a task that implements it.
- [ ] Integration tests cover all quickstart scenario steps.

## Constitutional Compliance
- [ ] Technology choices use only approved stack (constitution §Technology Guardrails).
- [ ] Quality thresholds met: test coverage target, documentation requirements.
- [ ] Workflow conventions followed: commit format, branching strategy.

## Internal Consistency
- [ ] Entity names and field names are consistent across `plan.md`, `tasks.md`, and `data-model.md`.
- [ ] CLI command signatures match between `plan.md` contracts and `tasks.md` criteria.
- [ ] Error behaviors described in `spec.md` edge cases appear in task acceptance criteria.

## Conflict Detection
- [ ] No requirement is defined in multiple places with inconsistent wording.
- [ ] No two tasks modify the same file with potentially conflicting changes.
- [ ] Dependency order in `tasks.md` has no cycles.

[Check every box before proceeding to implementation. For each unchecked box, update the upstream artifact until the check passes.]
```

---

### Note: Implementation Phase

The implementation phase does not produce a new specification artifact. It executes the validated `tasks.md` list, committing each completed task according to the workflow conventions in `constitution.md`.

**Loop-back reminder:** When implementation reveals that a spec assumption was wrong, a plan decision was flawed, or a task is missing — stop, update the upstream artifact, re-run analysis, and only then continue. Never silently diverge from the specification.
