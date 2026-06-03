# Task: <title>

> A single-concern unit of work. Author this **as a GitHub Issue** (label: `task`). Small enough to finish in one focused session and review in a few minutes.

**PRD:** #<prd-number>
**Wave:** <n>
**Depends on:** <Issue numbers, or "none">

## Goal

One sentence: what this task makes true that wasn't before.

## Acceptance criteria

The task is done when all of these hold. Keep them testable.

- [ ] Given ..., when ..., then ...
- [ ] All required [UX states](../../CONSTITUTION.md#ux-states-non-negotiable) handled (if this touches a UI surface).
- [ ] ...

## Tests

What proves it works. Every behavior change ships a test (see the [testing pyramid](../../CONSTITUTION.md#testing-pyramid-non-negotiable)).

- [ ] Unit: ...
- [ ] Integration: ... (if it crosses a real boundary)
- [ ] E2E: ... (only if it's a critical user journey)

## Files in scope

The files this task may touch. Keeps parallel tasks in the same wave from colliding.

- `path/to/file`
- ...

## Notes

Anything the implementer (human or agent) should know: gotchas, links, prior art.

---

*Implement in a [worktree](../process/spec-driven-workflow.md#4-implementation-developer--one-worktree-per-task) on `feat/<slug>`; open a small PR that `Closes #<this-issue>`.*
