---
description: Debug issues using the GSD scientific method with hypothesis testing
---

# GSD Debug — Systematic Bug Investigation

Investigate bugs using scientific method: observe, hypothesize, test, conclude. You are the investigator — the user is the reporter.

## Philosophy

**User = Reporter, You = Investigator**

The user knows: what they expected, what actually happened, error messages, when it started.
The user does NOT know (don't ask): what's causing the bug, which file has the problem, what the fix should be.

**Meta-debugging discipline:** When debugging code you wrote, treat it as foreign. Your mental model might be wrong — the code's behavior is truth.

## Cognitive Biases to Avoid

| Bias | Trap | Antidote |
|------|------|----------|
| Confirmation | Looking for evidence that supports your first guess | Actively try to DISPROVE your hypothesis |
| Anchoring | Fixating on the first error message | List ALL symptoms before forming hypothesis |
| Recency | Blaming the last change | Check git log, but verify causation |
| Complexity | Assuming a complex cause | Start with the simplest explanation |

## Step 1: Gather Symptoms

Ask the user (if not already provided):
1. What did you expect to happen?
2. What actually happened?
3. Any error messages? (exact text or screenshot)
4. When did it start? Did it ever work?

Do NOT ask about causes — that's your job.

## Step 2: Reproduce

Try to reproduce the issue:
```bash
# Run the relevant command/test
# Check logs
# Verify the error
```

If you can't reproduce: ask the user for exact steps, environment details.

## Step 3: Hypothesize

Form 2-3 ranked hypotheses based on symptoms:

```markdown
## Hypotheses (ranked by likelihood)

1. **{Most likely cause}** — Evidence: {what points to this}
2. **{Second possibility}** — Evidence: {what points to this}
3. **{Third possibility}** — Evidence: {what points to this}
```

## Step 4: Test Hypotheses

For each hypothesis, design a test that would DISPROVE it:

```bash
# H1 test: If {hypothesis}, then {observable consequence}
{test command}

# If test fails → hypothesis disproved, move to H2
# If test passes → hypothesis supported, investigate deeper
```

**Max 3 hypothesis cycles.** If still stuck after 3, reassess all assumptions.

## Step 5: Root Cause

When root cause is found:

```markdown
## Root Cause
**What:** {Precise technical description}
**Where:** {Exact file and line}
**Why:** {Why this causes the observed symptom}
**Evidence:** {What proved this is the cause}
```

## Step 6: Fix (if requested)

1. Implement the minimal fix that addresses the root cause.
2. Verify the fix resolves the original symptom.
3. Check for regressions (run related tests).
4. If the fix is complex, present options to the user before implementing.

## Step 7: Report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► DEBUG COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Root Cause: {one-line summary}
File: {path}:{line}
Fix: {applied | suggested}
Regression: {tests pass | needs verification}
```
