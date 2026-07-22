# OpenSpec + Superpowers Workflow Routing

Applies to any repo containing an `openspec/` directory. If there is no
`openspec/` directory, ignore this file entirely and use normal workflows.

## Before starting any development work

Run this check first:

    ls openspec/ 2>/dev/null && openspec list

If `openspec/` exists, load the `openspec-superpowers-bridge` skill and follow
it. Do this BEFORE reaching for any Superpowers skill.

Do NOT start with the Superpowers `brainstorming` skill. Design and
requirements are owned by OpenSpec (`/opsx:explore`, `/opsx:propose`) and live
in `openspec/changes/<name>/`.

## Implementation handoff

When a change is proposed and approved and it's time to build — including when
the user says "implement this", "build it", "let's do it", or similar:

Do NOT run `/opsx:apply`. Implementation is owned by Superpowers:

1. `using-git-worktrees` — branch named for the change, verify clean baseline
2. `writing-plans` — expand tasks.md into 2-5 minute steps
3. `subagent-driven-development` with `test-driven-development`
4. `requesting-code-review` against the delta specs in
   `openspec/changes/<name>/specs/`

OpenSpec's role resumes at close-out: check off tasks.md, then `/opsx:sync`,
then `openspec archive`.
