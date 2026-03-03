---
description: Check GSD project progress and get routed to the next action
---

# GSD Progress — Situational Awareness & Next Action

Check project progress, summarize recent work, and intelligently route to the next action.

## Step 1: Check Project Exists

1. Check if `.planning/` directory exists.
   - If no: "No planning structure found. Run `/gsd-new-project` to start."
2. Check if `.planning/STATE.md` exists.
3. Check if `.planning/ROADMAP.md` exists.
   - If no ROADMAP but PROJECT.md exists: Milestone was completed/archived. Suggest `/gsd-new-project` for next milestone.

## Step 2: Load State

Read `.planning/STATE.md` for:
- Current milestone and phase
- Status (planning, executing, verifying)
- Recent decisions

Read `.planning/ROADMAP.md` for:
- All phases with goals
- Dependencies between phases

## Step 3: Analyze Phase Status

For each phase in ROADMAP.md, scan `.planning/phases/{phase-dir}/`:
```
ls .planning/phases/*/  # List all phase directories and their contents
```

Classify each phase:
| Status | Meaning |
|--------|---------|
| **complete** | Has VERIFICATION.md with status: passed |
| **partial** | Has some SUMMARY.md but not all plans done |
| **planned** | Has PLAN.md files but no SUMMARY.md |
| **empty** | Directory exists but no plans |
| **not started** | No directory |

## Step 4: Identify Recent Work

Find the 2-3 most recent SUMMARY.md files (by modification date):
```bash
ls -t .planning/phases/*/*-SUMMARY.md 2>/dev/null | head -3
```
Read the "One-Liner" from each to show what was recently done.

## Step 5: Determine Next Action

Route based on current state:

| Situation | Route | Command |
|-----------|-------|---------|
| Phase has unexecuted plans | Execute next plan | `/gsd-execute {phase}` |
| Phase fully executed, not verified | Verify phase | `/gsd-verify {phase}` |
| Phase verified with gaps | Plan gap closure | `/gsd-plan {phase} --gaps` |
| Phase complete | Plan next phase | `/gsd-plan {next_phase}` |
| All phases complete | Complete milestone | Suggest next milestone |
| No plans exist for current phase | Create plans | `/gsd-plan {phase}` |

## Step 6: Present Report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PROJECT STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{Project Name} — Milestone v{X}

| Phase | Name | Status | Plans |
|-------|------|--------|-------|
| 1 | {name} | ✅ Complete | 3/3 |
| 2 | {name} | ⚡ Executing | 1/2 |
| 3 | {name} | 📋 Planned | 0/3 |
| 4 | {name} | — Not started | — |

Progress: {completed}/{total} phases ({percent}%)

Recent work:
  • {One-liner from most recent summary}
  • {One-liner from second most recent}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

▶ Next: {recommended command}
```
