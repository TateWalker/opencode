---
name: openspec-superpowers-bridge
description: Use at the START of any feature, bugfix, or development request in a repo containing an openspec/ directory. Routes design through OpenSpec (OPSX) and implementation through Superpowers. Load this BEFORE the Superpowers brainstorming skill.
---

# OpenSpec + Superpowers Bridge

This repo uses OpenSpec for specs and Superpowers for implementation.
OpenSpec owns **what and why**. Superpowers owns **how it gets built**.

## Detection

Before any development work, check for `openspec/`:

```bash
ls openspec/ 2>/dev/null && openspec list
```

If `openspec/` exists, this skill governs. If not, use the normal
Superpowers workflow unchanged.

## Division of responsibility

| Concern | Owner | Artifact |
|---|---|---|
| Idea exploration | OpenSpec | `/opsx:explore` |
| Proposal, design, tasks | OpenSpec | `openspec/changes/<name>/{proposal,design,tasks}.md` |
| Spec deltas | OpenSpec | `openspec/changes/<name>/specs/` |
| Isolated workspace | Superpowers | `using-git-worktrees` |
| Task decomposition | Superpowers | `writing-plans` |
| Execution | Superpowers | `subagent-driven-development` |
| Test discipline | Superpowers | `test-driven-development` |
| Code review | Superpowers | `requesting-code-review` |
| Spec sync + archive | OpenSpec | `/opsx:sync`, `openspec archive` |

## Rules

**Do NOT run the Superpowers `brainstorming` skill when an OpenSpec change
already exists.** Its output would compete with `proposal.md` and `design.md`.
Read those instead and treat them as the approved design.

**Do NOT author a separate design document.** If design work is needed, it
belongs in `openspec/changes/<name>/design.md` via the OPSX workflow, where it
is version-controlled and archived.

**Do NOT let `/opsx:apply` drive implementation directly.** Its task loop is
coarser than Superpowers' and does not enforce RED-GREEN-REFACTOR.

**Never edit `openspec/specs/` by hand.** Specs change only through delta
files and `/opsx:sync`.

## Workflow

### 1. Establish the change (OpenSpec)

If no change exists for this work:

- Unclear what to build -> `/opsx:explore`
- Clear enough -> `/opsx:propose`

Confirm the artifacts are complete before proceeding:

```bash
openspec status --change "<name>" --json
```

Stop and get human sign-off on `proposal.md` and `design.md`. Do not
proceed to implementation without it.

### 2. Isolate (Superpowers)

Invoke `using-git-worktrees`. Branch name should match the change name.
Verify a clean test baseline before writing anything.

### 3. Plan (Superpowers, sourced from OpenSpec)

Invoke `writing-plans`, but **do not re-derive requirements**. Inputs are:

- `openspec/changes/<name>/tasks.md` - the work breakdown
- `openspec/changes/<name>/design.md` - the technical approach
- `openspec/changes/<name>/specs/` - the behavior being added or changed

Expand each OpenSpec task into Superpowers-grade steps: 2-5 minutes each,
exact file paths, complete code, explicit verification. Keep a reference from
each plan step back to its originating OpenSpec task ID so traceability
survives.

If a task in `tasks.md` turns out to be underspecified, do not guess.
Return to OpenSpec and amend the change.

### 4. Execute (Superpowers)

Invoke `subagent-driven-development`. One fresh subagent per plan task, with
two-stage review: spec compliance first, then code quality.

Spec compliance means compliance with the **OpenSpec delta specs**, not with
the plan. The plan is a means; the spec is the contract.

`test-driven-development` applies throughout. No implementation code before a
failing test. Test discipline is owned by the Superpowers skill - do not
delegate it to a subagent.

Available subagents for delegation: `code-reviewer`, `security-reviewer`,
`build-error-resolver`, `refactor-cleaner`, `e2e-runner`, `database-reviewer`,
and the language-specific reviewers and build resolvers.

### 5. Check off (OpenSpec)

As each OpenSpec task completes and its tests pass, mark it `[x]` in
`tasks.md`. Commit alongside the code, not in a batch at the end.

### 6. Close out

1. `requesting-code-review` against the delta specs

2. Security check - run before syncing specs:

   ```bash
   npm audit --audit-level=high     # or the ecosystem equivalent
   npx gitleaks detect --no-banner
   semgrep --config=auto <changed paths>
   ```

   Then delegate to the `security-reviewer` subagent with the diff and the
   delta specs from `openspec/changes/<name>/specs/`. Findings block archive
   until resolved or explicitly waived by the user.

3. `/opsx:sync` to merge spec updates into `openspec/specs/`

4. `openspec archive <name> --yes` (add `--skip-specs` for tooling-only
   changes)

5. `finishing-a-development-branch` - merge/PR decision, clean up the worktree

Archive only when `openspec status` shows all artifacts done and every task
checked.

## Small changes

For a typo, a one-line fix, or anything with no spec impact: skip OpenSpec
entirely, but still apply `test-driven-development` and
`verification-before-completion`. A change that needs a spec is a change that
needs a proposal.
