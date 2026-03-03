---
description: Verify a GSD phase achieved its goal through goal-backward analysis
---

# GSD Verify — Goal-Backward Phase Verification

Verify that a phase achieved its GOAL, not just completed its TASKS. Task completion ≠ Goal achievement.

## Core Principle

A task "create chat component" can be marked complete when the component is a placeholder. The task was done — but the goal "working chat interface" was not achieved.

Goal-backward verification:
1. What must be **TRUE** for the goal to be achieved?
2. What must **EXIST** for those truths to hold?
3. What must be **WIRED** for those artifacts to function?

Then verify each level against the actual codebase. **Do NOT trust SUMMARY.md claims.** Summaries document what was SAID was done. You verify what ACTUALLY exists.

## Step 1: Load Context

1. Read `.planning/ROADMAP.md` — extract the phase **goal** (the outcome to verify).
2. Read `.planning/REQUIREMENTS.md` — extract requirement IDs mapped to this phase.
3. List all `*-PLAN.md` and `*-SUMMARY.md` files in the phase directory.
4. Check for existing `*-VERIFICATION.md` (re-verification mode if gaps exist).

## Step 2: Establish Must-Haves

Extract `must_haves` from PLAN.md frontmatter if available:
```yaml
must_haves:
  truths: ["User can log in with Google"]
  artifacts: ["app/(auth)/login.tsx"]
  key_links: ["GoogleSignin.configure() called with correct clientId"]
```

If no must_haves in frontmatter, derive from phase goal:
- **Truths:** What observable behaviors must exist?
- **Artifacts:** What concrete files/endpoints must exist?
- **Key Links:** What critical wiring connects artifacts to make truths hold?

## Step 3: Three-Level Verification

For each must-have, verify at three levels:

### Level 1: Existence
Does the artifact exist?
```bash
# Check files exist
ls -la {artifact_path}
```

### Level 2: Substance
Is it real implementation, not a stub/placeholder?
- Read each artifact file.
- Check for TODO comments, placeholder text, empty functions, hardcoded mock data.
- A 10-line component with `// TODO: implement` is a stub, not substance.

### Level 3: Wiring
Are artifacts connected to the rest of the system?
- Check imports: Is the component imported where it should be?
- Check routing: Is the page accessible via navigation?
- Check data flow: Does the API endpoint connect to the database? Does the UI call the API?
- Run automated checks if available:
```bash
# Example: grep for imports
grep -r "import.*{ComponentName}" src/
```

## Step 4: Run Automated Verification

For each plan's `<verify>` commands, re-run them:
```bash
# Run each plan's verification commands
{verify_command_from_plan}
```

Also run project-level checks:
```bash
# Type checking
npx tsc --noEmit 2>&1 | head -20

# Lint
npx eslint . --max-warnings=0 2>&1 | tail -10

# Tests
npm test 2>&1 | tail -20
```

## Step 5: Write Verification Report

Create `.planning/phases/{phase-dir}/{phase}-VERIFICATION.md`:

```markdown
---
phase: {number}
status: {passed | failed}
verified: {ISO date}
gaps: {list of failed items, or empty}
---

# Phase {X} Verification: {Phase Name}

## Goal
{Phase goal from ROADMAP.md}

## Must-Haves Verification

### Truths
| # | Truth | Exists | Substance | Wired | Status |
|---|-------|--------|-----------|-------|--------|
| 1 | {truth} | ✅ | ✅ | ✅ | PASS |
| 2 | {truth} | ✅ | ❌ stub | — | FAIL |

### Artifacts
| File | Exists | Real | Connected |
|------|--------|------|-----------|
| {path} | ✅ | ✅ | ✅ |

### Key Links
| Link | Status | Evidence |
|------|--------|----------|
| {description} | ✅ | {grep/test result} |

## Automated Checks
| Check | Result |
|-------|--------|
| TypeScript | {pass/fail} |
| Lint | {pass/fail} |
| Tests | {pass/fail} |

## Gaps (if any)
{Detailed description of what failed and why}

## Verdict
**{PASSED | FAILED — N gaps found}**
```

## Step 6: Report Results

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PHASE {X} VERIFICATION: {PASSED | FAILED}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{N}/{total} must-haves verified

Gaps: {None | list}

Next: {/gsd-plan {X} --gaps (if failed) | /gsd-plan {X+1} (if passed)}
```
