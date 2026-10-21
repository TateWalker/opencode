---
description: Fix several Linear issues in parallel with subagents (plan, approve, implement, merge)
agent: build
---
Fix these Linear issues in parallel: **$ARGUMENTS**

You are the coordinator. Subagents do the investigation and the coding. You manage worktrees, gather questions, and merge. Follow the phases in order, and stop at each **STOP**.

Repo state now:
!`git status --short --branch`

## Phase 1: Plan (read-only)

1. If the working tree above has uncommitted changes, or you're not on `develop`, stop and tell me.
2. Start one `issue-scout` subagent **per issue, all in parallel**. Pass each one only its issue ID.
3. Check the plans for **overlaps**: the same files, or what looks like the same root cause. Issues that overlap must not be fixed in parallel. Propose merging them into one fix, or doing them one after another.
4. Present:
   - One section per issue: its plan, size, and confidence
   - **All questions for me, in one numbered list**, each tagged with its issue ID
   - Overlaps and what you propose to do about them
   - Issues you recommend skipping (L size, low confidence, or blocked), and why

**STOP.** Wait for my answers and my approval. Only the issues I approve go on to Phase 2.

## Phase 2: Implement (parallel)

1. For each approved ID, create an isolated worktree from the up-to-date `develop`, and mark the issue started:
   ```
   git fetch origin
   grep -qxF '.worktrees/' .git/info/exclude || echo '.worktrees/' >> .git/info/exclude
   git worktree add --detach .worktrees/<ID> origin/develop
   (cd .worktrees/<ID> && linear issue start <ID>)
   ```
   If any of these fail for an issue, drop that issue and tell me why. Don't improvise around the failure.
2. Start one `issue-fixer` subagent **per approved issue, all in parallel**. Give each one:
   - The issue ID
   - The **absolute path** of its worktree
   - The approved plan, plus my answers to its questions
3. When they're all done, check every worktree with `git -C .worktrees/<ID> status --short` and `git -C .worktrees/<ID> log --oneline origin/develop..HEAD`. Also check the **main checkout** with `git status --short`. If anything changed in the main checkout, a fixer escaped its worktree. Report it and do not merge.
4. Present one row per issue: what changed, which files, whether tests passed, and anything the fixer flagged.

**STOP.** Wait for my approval of which branches to merge.

## Phase 3: Merge (sequential)

For each approved branch, one at a time, in the order I give (or smallest first):
1. `git switch develop && git pull --ff-only`
2. `git merge --no-ff <branch>`
3. Run the project's tests and build. If the merge conflicts or tests fail, run `git merge --abort` (or reset to before the merge if already committed), stop, and tell me. Don't resolve conflicts on your own.

When everything has merged cleanly:
- Remove the merged worktrees: `git worktree remove .worktrees/<ID>`
- Tell me what merged and the commit range.

**Do not push.** Do not move any issue to Done. I do both.
