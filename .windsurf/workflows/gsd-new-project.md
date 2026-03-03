---
description: Initialize a new GSD project with questioning, requirements, and roadmap
---

# GSD New Project — Initialize from Idea to Ready-for-Planning

This is the most leveraged moment in any project. Deep questioning here means better plans, better execution, better outcomes. One workflow takes you from idea to ready-for-planning.

## Step 1: Check Existing State

1. Check if `.planning/` directory already exists.
   - If yes: Tell user "Project already initialized. Use `/gsd-progress` to check status."
   - If no: Continue.
2. Create `.planning/` directory.

## Step 2: Brownfield Detection

Search the workspace for existing code:
- Check for `package.json`, `requirements.txt`, `go.mod`, `Cargo.toml`, etc.
- Check for `src/`, `app/`, `lib/` directories with code files.

If existing code found, ask the user:
- **"Map codebase first"** — Scan the codebase to understand existing architecture before planning (recommended for brownfield).
- **"Skip mapping"** — Proceed as if greenfield.

If mapping: Run `/gsd-map-codebase` logic (scan dirs, identify stack, write `.planning/CODEBASE.md`).

## Step 3: Deep Questioning

Ask the user about their project. These questions extract the context needed for great plans:

**Round 1 — The Idea:**
1. What are you building? (1-2 sentence elevator pitch)
2. Who is it for? (target user)
3. What's the single most important thing it must do?

**Round 2 — Scope & Constraints:**
4. What's your timeline? (MVP in days/weeks/months)
5. What tech stack? (or "you decide")
6. Any hard constraints? (must use X, can't use Y, budget limits)

**Round 3 — Depth:**
7. How thorough should planning be?
   - **Quick** — Ship fast (3-5 phases, 1-3 plans each)
   - **Standard** — Balanced (5-8 phases, 3-5 plans each)
   - **Comprehensive** — Thorough (8-12 phases, 5-10 plans each)

If the user provides a PRD or idea document instead of answering questions, extract all answers from the document.

## Step 4: Write PROJECT.md

Create `.planning/PROJECT.md` with:
```markdown
# {Project Name}

## Vision
{Elevator pitch}

## Target User
{Who it's for}

## Core Value
{Single most important thing}

## Tech Stack
{Stack decisions}

## Constraints
{Hard constraints}

## Planning Depth
{quick | standard | comprehensive}

---
*Initialized: {date}*
```

## Step 5: Research (Optional)

If the project involves unfamiliar tech or complex architecture:
- Research best practices, common pitfalls, library choices.
- Write `.planning/RESEARCH.md` with findings.

Ask user: "Should I research the tech stack before creating the roadmap?" (default: yes for unfamiliar stacks)

## Step 6: Generate Requirements

Create `.planning/REQUIREMENTS.md`:

```markdown
# Requirements

| ID | Category | Requirement | Priority | Phase |
|----|----------|-------------|----------|-------|
| REQ-01 | Auth | User can sign up with email/password | Must | 1 |
| REQ-02 | Auth | User can sign in with Google SSO | Must | 1 |
| REQ-03 | Core | User can create a new project | Must | 2 |
```

Categories: Auth, Core, UI/UX, Data, Integration, Performance, Security
Priorities: Must (MVP), Should (v1.1), Could (backlog)

Present to user for approval. Revise based on feedback.

## Step 7: Generate Roadmap

Create `.planning/ROADMAP.md`:

```markdown
# Roadmap — {Project Name}

## Milestone: v{X} — {Milestone Name}

### Phase 1: {Name}
**Goal:** {One sentence — what's TRUE when this phase is done}
**Dependencies:** None
**Requirements:** REQ-01, REQ-02
**Success Criteria:**
- [ ] {Observable, testable behavior}
- [ ] {Observable, testable behavior}

### Phase 2: {Name}
**Goal:** {Goal}
**Dependencies:** Phase 1
**Requirements:** REQ-03, REQ-04
**Success Criteria:**
- [ ] {Criteria}
```

Rules:
- Each phase has a clear **goal** (not a list of tasks).
- Dependencies are explicit.
- Success criteria are observable and testable.
- Phases are ordered by dependency, not priority.

Present to user for approval. Revise based on feedback.

## Step 8: Initialize State

Create `.planning/STATE.md`:

```markdown
# Project State

## Current Position
**Milestone:** v{X} — {Name}
**Phase:** Not started
**Status:** Ready for planning

## Decisions Log
| Date | Decision | Context |
|------|----------|---------|

## Quick Tasks Completed
| # | Date | Description | Status |
|---|------|-------------|--------|
```

## Step 9: Present Summary

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PROJECT INITIALIZED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{Project Name} — {N} phases, {M} requirements

Files created:
  .planning/PROJECT.md
  .planning/REQUIREMENTS.md
  .planning/ROADMAP.md
  .planning/STATE.md

Next: /gsd-plan 1
```
