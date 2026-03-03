---
description: Show all available GSD commands and how to use them
---

# GSD Help — Command Reference

Display the following help text to the user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► GET SHIT DONE — Windsurf Edition
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

A spec-driven development system adapted from
github.com/gsd-build/get-shit-done for Windsurf.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 COMMANDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROJECT LIFECYCLE
  /gsd-new-project    Initialize project (questioning → requirements → roadmap)
  /gsd-progress       Check status and get routed to next action

PLANNING
  /gsd-plan {phase}   Create executable PLAN.md files for a phase
                      Options: --skip-research, --gaps

EXECUTION
  /gsd-execute {phase} Execute plans task-by-task with verification
  /gsd-quick {task}    Quick ad-hoc task with GSD quality guarantees

VERIFICATION
  /gsd-verify {phase}  Goal-backward verification (did we achieve the goal?)

DEBUGGING
  /gsd-debug           Scientific method bug investigation

HELP
  /gsd-help            This help text

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 TYPICAL WORKFLOW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. /gsd-new-project      → Describe idea, get roadmap
2. /gsd-plan 1            → Create plans for Phase 1
3. /gsd-execute 1         → Build Phase 1
4. /gsd-verify 1          → Verify Phase 1 goal achieved
5. /gsd-plan 2            → Plan next phase
6. ... repeat until done

For quick fixes: /gsd-quick "fix the login button"
For bugs:        /gsd-debug

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 FILE STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

.planning/
├── PROJECT.md              Project vision & constraints
├── REQUIREMENTS.md         Tracked requirements (REQ-XX)
├── ROADMAP.md              Phases with goals & dependencies
├── STATE.md                Current position & decisions
├── phases/
│   ├── 01-phase-name/
│   │   ├── 01-01-PLAN.md      Execution plan
│   │   ├── 01-01-SUMMARY.md   Execution results
│   │   ├── 01-RESEARCH.md     Technical research
│   │   └── 01-VERIFICATION.md Goal verification
│   └── 02-phase-name/
│       └── ...
└── quick/
    ├── 001-fix-thing/
    │   ├── PLAN.md
    │   └── SUMMARY.md
    └── ...
```
