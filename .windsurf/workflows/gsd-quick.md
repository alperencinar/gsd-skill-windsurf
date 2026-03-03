---
description: Execute a quick ad-hoc task with GSD quality guarantees
---

# GSD Quick — Fast Ad-Hoc Tasks with Structure

Execute small, ad-hoc tasks with GSD guarantees: structured plan, atomic implementation, state tracking. For tasks that don't warrant a full phase.

## Step 1: Get Task Description

If the user didn't provide a description, ask: "What do you want to do?"

The description should be a clear, concise statement of the task.

## Step 2: Initialize

1. Ensure `.planning/` exists (if not: "Run `/gsd-new-project` first").
2. Create `.planning/quick/` directory if it doesn't exist.
3. Determine next task number:
   ```bash
   ls .planning/quick/ 2>/dev/null | wc -l
   ```
4. Create task directory: `.planning/quick/{number}-{slug}/`

## Step 3: Create Quick Plan

Write a lightweight PLAN.md in the task directory:

```markdown
---
type: quick
description: {task description}
created: {ISO date}
---

# Quick Task: {Description}

## Tasks

<task type="auto">
  <name>{Task name}</name>
  <files>{exact files to create/modify}</files>
  <action>{specific implementation instructions}</action>
  <verify>{automated verification command}</verify>
  <done>{measurable completion criteria}</done>
</task>
```

Quick plans follow the same task anatomy rules as full plans:
- Specific file paths (not "the relevant files")
- Concrete action instructions (not "make it work")
- Automated verify command
- Measurable done criteria

## Step 4: Execute

For each task:
1. Implement the `<action>`.
2. Run the `<verify>` command.
3. Confirm `<done>` criteria met.

## Step 5: Write Summary

Write SUMMARY.md in the task directory:

```markdown
---
type: quick
status: complete
completed: {ISO date}
---

# Quick Task Complete: {Description}

## What Changed
{List of files created/modified with brief description}

## Verification
{Result of verify command}
```

## Step 6: Update State

Append to the "Quick Tasks Completed" table in `.planning/STATE.md`:

```markdown
| {number} | {date} | {description} | ✅ Complete |
```

## Step 7: Report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► QUICK TASK COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Task #{number}: {description}
Files: {count} modified
Status: ✅ Complete
```
