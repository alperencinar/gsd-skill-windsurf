---
description: Execute a GSD phase plan (PLAN.md) with task-by-task implementation
---

# GSD Execute — Run Plans with Atomic Implementation

Execute PLAN.md files task-by-task. For each task: implement, verify, commit. Produce SUMMARY.md when done.

## Step 1: Load Context

1. Read `.planning/STATE.md` for project position and decisions.
2. Identify the target phase from user input or STATE.md's current position.
3. List plans in `.planning/phases/{phase-dir}/`:
   ```
   ls .planning/phases/{phase-dir}/*-PLAN.md
   ls .planning/phases/{phase-dir}/*-SUMMARY.md
   ```
4. Find the first PLAN.md without a matching SUMMARY.md — that's the next plan to execute.
5. If all plans have summaries, report "All plans executed for this phase" and suggest `/gsd-verify`.

## Step 2: Parse Plan

Read the PLAN.md file. Extract:
- **Frontmatter:** phase, plan, type, wave, depends_on, files_modified, autonomous, requirements, must_haves
- **Objective:** What we're building and why
- **Context:** File references to read before executing
- **Tasks:** Each with files, action, verify, done
- **Success Criteria:** Phase-level acceptance

Read ALL files referenced in the Context section before starting any task.

## Step 3: Check Dependencies

If `depends_on` lists other plans, verify those plans have SUMMARY.md files (meaning they're complete).
- If dependencies not met: "Plan {X} depends on plans {Y} which aren't complete yet. Execute those first."

## Step 4: Execute Tasks

For each task in order:

### 4a. Implement
- Read the `<action>` instructions carefully.
- If `tdd="true"`: Write tests first (RED), then implement (GREEN), then refactor.
- Create/modify only the files listed in `<files>`.
- Follow existing codebase patterns and conventions.
- If you need to deviate from the plan (e.g., a file path is wrong, an API changed):
  - **Minor deviation:** Fix and note in summary.
  - **Major deviation:** Stop, explain the issue to the user, ask how to proceed.

### 4b. Verify
- Run the `<verify>` command.
- If verification fails: debug, fix, re-run. Max 3 attempts.
- If still failing after 3 attempts: report the failure to the user with diagnostics.

### 4c. Confirm Done
- Check each criterion in `<done>`.
- Mark task as complete.

### 4d. Checkpoint Handling
- `checkpoint:human-verify`: Show the user what was built, ask them to verify visually/functionally. Wait for confirmation.
- `checkpoint:decision`: Present the decision needed with options. Wait for user choice.

## Step 5: Create SUMMARY.md

After all tasks complete, write `.planning/phases/{phase-dir}/{phase}-{plan}-SUMMARY.md`:

```markdown
---
phase: {number}
plan: {number}
status: complete
started: {ISO timestamp}
completed: {ISO timestamp}
tasks_completed: {N}/{total}
---

# Summary: {Plan Objective}

## One-Liner
{What was accomplished in one sentence}

## Tasks Completed

### Task 1: {Name}
- **Files:** {list}
- **Status:** Complete
- **Deviations:** {None | description}
- **Verification:** {Pass | details}

### Task 2: {Name}
...

## Deviations from Plan
{None, or describe what changed and why}

## Files Created/Modified
{Complete list of all files touched}

## Next Steps
{What should happen next — next plan, verification, etc.}
```

## Step 6: Update State

Update `.planning/STATE.md`:
- Update current position (which plan was just completed).
- Add any decisions made during execution to the Decisions Log.

## Step 7: Report & Next

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PLAN {X}-{Y} EXECUTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{Tasks completed}/{Total tasks} — {One-liner summary}
Deviations: {None | count}

Next plan: {next plan path, or "Phase complete — run /gsd-verify"}
```
