---
name: Task
about: A single-concern unit of work derived from a PRD.
title: "[wave <n>] <task title>"
labels: [task]
---

<!--
This mirrors docs/templates/task-template.md. Small enough to finish in one
focused session and review in a few minutes.
-->

**PRD:** #<prd-number>
**Wave:** <n>
**Depends on:** <issue numbers, or "none">

## Goal

<!-- One sentence: what this makes true that wasn't before. -->

## Acceptance criteria

- [ ] Given ..., when ..., then ...
- [ ] All required UX states handled (if this touches a UI surface).

## Tests

<!-- Every behavior change ships a test. -->

- [ ] Unit:
- [ ] Integration: <!-- if it crosses a real boundary -->
- [ ] E2E: <!-- only if it's a critical user journey -->

## Files in scope

<!-- Keeps parallel tasks in the same wave from colliding. -->

- `path/to/file`

## Notes

<!-- Gotchas, links, prior art for the implementer (human or agent). -->

---

*Implement in a worktree on `feat/<slug>`; open a small PR that `Closes` this Issue.*
