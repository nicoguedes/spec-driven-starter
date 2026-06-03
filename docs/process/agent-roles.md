# Agent roles

Five roles move work through the [lifecycle](../../CONSTITUTION.md#the-spec-driven-lifecycle). A role is a *responsibility*, not a person — one human or agent can wear several, and on a small change one actor may wear all of them. Naming them explicitly is what lets us hand work off cleanly between humans and agents, and between agents working in parallel.

The roles: **Orchestrator**, **Product Manager**, **Developer**, **QA**, **PR Reviewer / Merger**.

---

## Orchestrator

- **Mission** — Turn a PRD into a correctly-sequenced plan and keep the waves moving. Owns the dependency graph, not the code.
- **Inputs** — An approved PRD Issue; the current state of open Issues and PRs.
- **Outputs** — A task breakdown grouped into waves; one Issue per task with dependencies recorded; assignments of tasks to Developers (human or agent).
- **Does** — Decompose into small single-concern tasks; identify dependencies; group independent tasks into parallel waves; dispatch a wave only when its prerequisite wave is merged; unblock and re-sequence when reality diverges from the plan.
- **Does NOT** — Write feature code; approve or merge PRs; redefine product scope (that's the PM's call — escalate instead).

## Product Manager

- **Mission** — Own the *what* and the *why*. Make intent explicit and agreed before implementation starts.
- **Inputs** — A raw idea or problem statement; user/stakeholder context.
- **Outputs** — A PRD Issue ([template](../templates/prd-template.md)) with crisp acceptance criteria, scope, non-goals, and required UX states.
- **Does** — Frame the problem and the users; set scope and explicit non-goals; define acceptance criteria that are testable; sign off when a delivered change meets the spec.
- **Does NOT** — Prescribe implementation details (the *how*); break work into tasks (that's the Orchestrator); approve the technical correctness of a PR.

## Developer

- **Mission** — Implement one task to its acceptance criteria, with tests, in isolation.
- **Inputs** — One task Issue ([template](../templates/task-template.md)) with acceptance criteria and a list of files in scope; the [Constitution](../../CONSTITUTION.md) and [`CLAUDE.md`](../../CLAUDE.md).
- **Outputs** — A small, single-concern PR on a feature branch in its own worktree, with tests, that closes its Issue and passes CI.
- **Does** — Build exactly what the Issue specifies; write the tests that prove it; handle the required UX states; keep the diff scoped; respond to review.
- **Does NOT** — Expand scope beyond the Issue (file a new Issue instead); touch files owned by a sibling task in the same wave; merge its own PR.

## QA

- **Mission** — Verify the delivered change against the spec, beyond what automated tests cover.
- **Inputs** — A merged/deployed change; the PRD's acceptance criteria; a test plan ([template](../templates/test-plan-template.md)).
- **Outputs** — A pass/fail result on the test plan; follow-up Issues for every defect or gap found.
- **Does** — Execute the manual test plan; probe edge cases and the four UX states; confirm acceptance criteria hold in the real deployed environment.
- **Does NOT** — Fix the bugs it finds (file Issues; a Developer picks them up); approve merges.

## PR Reviewer / Merger

- **Mission** — Be the gate. Nothing reaches `main` without passing through here.
- **Inputs** — An open PR with green CI, linked to an Issue.
- **Outputs** — Approval and a squash-merge, or change requests.
- **Does** — Verify the PR is single-concern; confirm acceptance criteria and the [Definition of Done](../../CONSTITUTION.md#definition-of-done); read the tests as carefully as the code; require human approval before merge; squash-merge so it auto-deploys.
- **Does NOT** — Rubber-stamp; merge without explicit human approval; merge a PR that bundles unrelated changes.

---

## Handoffs

```
  PRODUCT MANAGER ──── PRD Issue ────▶ ORCHESTRATOR
                                            │
                                   task Issues, in waves
                                            │
                                            ▼
                                       DEVELOPER  ◀── (one per Issue, parallel within a wave)
                                            │
                                       PR (green CI)
                                            │
                                            ▼
                              PR REVIEWER / MERGER ──── approve + squash-merge ──▶ auto-deploy
                                            │
                                            ▼
                                          QA  ──── defects? ──▶ new Issues ──▶ ORCHESTRATOR
                                            │
                                          pass
                                            │
                                            ▼
                                          DONE  ──── PM signs off against acceptance criteria
```

The only loop is QA → Issues → Orchestrator, which is by design: found defects re-enter the same disciplined pipeline rather than being patched out-of-band.

See the [operational playbook](spec-driven-workflow.md) for the exact commands each role runs.
