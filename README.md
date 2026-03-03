# GSD for Windsurf — Get Shit Done Planning System

A spec-driven development workflow system for [Windsurf](https://windsurf.ai) that brings structured planning, execution, and verification to your projects. Adapted from [get-shit-done](https://github.com/gsd-build/get-shit-done).

## What This Is

**GSD (Get Shit Done)** is a meta-prompting system that prevents quality degradation during AI-assisted development. It structures your work into phases, plans, and tasks — each with clear goals, verification, and state tracking.

Instead of telling your AI "build me a login page" and hoping for the best, GSD gives you:

- **Roadmaps** with phased goals and dependencies
- **Executable plans** with specific task anatomy (files, action, verify, done)
- **Goal-backward verification** — did we achieve the goal, not just complete the tasks?
- **State tracking** across sessions via `.planning/` directory

## Quick Install

**Option A — Copy the `workflows/` folder into your project:**

```bash
# From your project root:
mkdir -p .windsurf/workflows
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-plan.md -o .windsurf/workflows/gsd-plan.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-execute.md -o .windsurf/workflows/gsd-execute.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-verify.md -o .windsurf/workflows/gsd-verify.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-new-project.md -o .windsurf/workflows/gsd-new-project.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-quick.md -o .windsurf/workflows/gsd-quick.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-progress.md -o .windsurf/workflows/gsd-progress.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-debug.md -o .windsurf/workflows/gsd-debug.md
curl -sL https://raw.githubusercontent.com/<owner>/<repo>/main/.windsurf/workflows/gsd-help.md -o .windsurf/workflows/gsd-help.md
```

**Option B — Tell your Windsurf agent:**

> "Download the `.windsurf/workflows/` folder from `https://github.com/<owner>/<repo>` and place it in my project's `.windsurf/workflows/` directory."

**Option C — Clone and copy:**

```bash
git clone https://github.com/<owner>/<repo> /tmp/gsd-windsurf
cp -r /tmp/gsd-windsurf/.windsurf/workflows/ .windsurf/workflows/
rm -rf /tmp/gsd-windsurf
```

## Commands

Once installed, use these slash commands in Windsurf chat:

| Command | What it does |
|---------|-------------|
| `/gsd-help` | Show all commands and usage guide |
| `/gsd-new-project` | Initialize a project (idea → requirements → roadmap) |
| `/gsd-plan {phase}` | Create executable plans for a phase |
| `/gsd-execute {phase}` | Execute plans task-by-task |
| `/gsd-verify {phase}` | Goal-backward verification |
| `/gsd-progress` | Check status, get routed to next action |
| `/gsd-quick {task}` | Quick ad-hoc task with quality guarantees |
| `/gsd-debug` | Scientific method bug investigation |

## Typical Workflow

```
1. /gsd-new-project        → Describe your idea, get a roadmap
2. /gsd-plan 1             → Create plans for Phase 1
3. /gsd-execute 1          → Build Phase 1
4. /gsd-verify 1           → Verify Phase 1 goal achieved
5. /gsd-plan 2             → Plan next phase
6. ... repeat until done
```

For quick fixes anytime: `/gsd-quick "fix the login button"`
For bugs: `/gsd-debug`

## File Structure

GSD creates a `.planning/` directory in your project root to track all state:

```
.planning/
├── PROJECT.md              # Project vision & constraints
├── REQUIREMENTS.md         # Tracked requirements (REQ-XX)
├── ROADMAP.md              # Phases with goals & dependencies
├── STATE.md                # Current position & decisions
├── phases/
│   ├── 01-phase-name/
│   │   ├── 01-01-PLAN.md       # Execution plan
│   │   ├── 01-01-SUMMARY.md    # Execution results
│   │   ├── 01-RESEARCH.md      # Technical research
│   │   └── 01-VERIFICATION.md  # Goal verification report
│   └── 02-phase-name/
│       └── ...
└── quick/
    └── 001-fix-thing/
        ├── PLAN.md
        └── SUMMARY.md
```

## Key Concepts

### Plans Are Prompts
A `PLAN.md` is not a document — it IS the execution prompt. It contains everything the AI needs to implement without asking clarifying questions.

### Task Anatomy
Every task has four required fields:
- **`<files>`** — Exact file paths (not "the auth files")
- **`<action>`** — Specific instructions with rationale (not "add authentication")
- **`<verify>`** — Automated bash command to prove completion
- **`<done>`** — Measurable acceptance criteria

### Goal-Backward Verification
Completing tasks ≠ achieving goals. Verification checks:
1. What must be **TRUE** for the goal to be achieved?
2. What must **EXIST** for those truths to hold?
3. What must be **WIRED** for those artifacts to function?

### Wave-Based Execution
Plans are grouped into dependency waves for optimal execution order. Independent plans run in the same wave; dependent plans wait.

## For AI Agents — Auto-Setup Instructions

If you are a coding agent asked to install this skill:

1. **Download** all `.md` files from this repository's `.windsurf/workflows/` directory.
2. **Place** them in the user's project at `.windsurf/workflows/`.
3. **Verify** the files exist:
   ```bash
   ls .windsurf/workflows/gsd-*.md
   ```
4. **Tell the user** they can now use `/gsd-help` to see all available commands, and `/gsd-new-project` to start planning their project.

No dependencies required. No build step. Just markdown workflow files.

## Credits

Adapted from the [Get Shit Done](https://github.com/gsd-build/get-shit-done) system by TÂCHES, originally built for Claude Code. This Windsurf adaptation preserves the core methodologies (task anatomy, goal-backward verification, wave-based execution, dependency graphs) while adapting to Windsurf's single-agent workflow model.

## License

MIT
