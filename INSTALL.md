# GSD Windsurf Skill — Agent Installation Guide

This file is specifically for AI coding agents that are asked to install this skill.

## Auto-Install Steps

When a user asks you to install this GSD skill into their project, follow these steps:

### 1. Create the target directory

```bash
mkdir -p .windsurf/workflows
```

### 2. Copy all workflow files

Copy every `gsd-*.md` file from this repository's `.windsurf/workflows/` directory into the user's `.windsurf/workflows/` directory:

| File | Purpose |
|------|---------|
| `gsd-plan.md` | Create executable phase plans |
| `gsd-execute.md` | Execute plans task-by-task |
| `gsd-verify.md` | Goal-backward phase verification |
| `gsd-new-project.md` | Initialize project from idea to roadmap |
| `gsd-quick.md` | Quick ad-hoc tasks |
| `gsd-progress.md` | Check status, route to next action |
| `gsd-debug.md` | Scientific method debugging |
| `gsd-help.md` | Command reference |

### 3. Verify installation

```bash
ls .windsurf/workflows/gsd-*.md | wc -l
# Should output: 8
```

### 4. Tell the user

After installation, inform the user:

```
GSD Windsurf skill installed successfully (8 workflow files).

Available commands:
  /gsd-help           — See all commands
  /gsd-new-project    — Start a new project
  /gsd-plan {phase}   — Plan a phase
  /gsd-execute {phase} — Execute a phase
  /gsd-verify {phase}  — Verify a phase
  /gsd-progress       — Check project status
  /gsd-quick {task}   — Quick task
  /gsd-debug          — Debug an issue

Start with: /gsd-new-project
```

## What NOT to do

- Do NOT install any npm packages — this skill has zero dependencies.
- Do NOT modify the user's existing `.windsurf/` files — only add new `gsd-*.md` files.
- Do NOT create a `.planning/` directory — that's created by `/gsd-new-project` when the user is ready.
- Do NOT copy `README.md`, `INSTALL.md`, or `gsd-skill.json` — those are documentation only.
