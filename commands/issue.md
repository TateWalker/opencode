---
description: Start work on a Linear issue (branch, mark started, plan; no code edits)
agent: plan
---
Starting Linear issue **$1**.

Working tree before starting:
!`git status --short --branch`

Branch + status (ran `linear issue start $1`):
!`linear issue start $1 2>&1`

The issue:
!`linear issue view $1 2>&1`

Do these in order:

1. **Sanity check.** If `linear issue start` errored, or the working tree above had uncommitted changes, stop and tell me. Don't stash, reset, or discard anything.

2. **Read the whole issue.** If it references other issues, or the description leaves you without enough context, use the `linear` CLI (see the linear-cli skill) to read related issues and the comments on $1.

3. **Investigate before proposing.** Find the relevant code. Reproduce the bug from the report if you can, or explain why you can't.

4. **Write the plan.** Include:
   - What's wrong (or what's being added), in one or two sentences
   - The root cause, with file and line references, or the most likely cause and what would confirm it
   - The change: which files to touch and the approach
   - How to verify it: a test or manual steps
   - Open questions, or anything in the report that's ambiguous

Do **not** edit code until I approve the plan. If the issue is too vague to plan, say what's missing and draft a comment I could post asking the reporter.
