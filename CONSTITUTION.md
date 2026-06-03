# Constitution

This is the highest-level "how we work" document for projects built from this template. It governs both humans and agents. When anything below conflicts with convenience, the Constitution wins.

If you are an automated agent: read this file and [`CLAUDE.md`](CLAUDE.md) before doing anything else.

---

## Prime directives

1. **Velocity first — but no compliance theater.** Optimize for shipping working software, not for ceremony. Every rule here exists because it makes us *faster over the next month*, not because it looks rigorous. If a step is pure overhead, cut it. The gates below are the deliberate exceptions: they are cheap and they are non-negotiable.
2. **Never push to `main`.** All changes land through a pull request. No exceptions, including for "tiny" fixes.
3. **Never merge without explicit human approval.** Agents may open PRs, respond to review, and make CI green. A human performs the merge. This is the single irreducible human-in-the-loop step.
4. **Small, single-concern PRs.** One PR does one thing. If you can't describe the change in a sentence without "and," split it. Small PRs review faster, revert cleanly, and parallelize well.
5. **Every behavior change ships a test.** New behavior or a bug fix arrives with a test that fails before the change and passes after. Refactors that change no behavior may rely on existing coverage.
6. **Secrets live only in a secret manager.** Never commit a credential, token, or key — not in code, not in config, not in CI YAML. CI and runtime read secrets from the platform's secret store / repository secrets. A committed secret is a stop-the-line incident.

---

## The spec-driven lifecycle

We agree on the *what* and *why* in writing before the *how*. Intent lives in **GitHub Issues**, not in stale repo docs.

```
IDEA → PRD → TASK BREAKDOWN (waves) → ISSUES → IMPLEMENTATION (parallel agents) → REVIEW/MERGE → QA → DONE
```

- **PRD** — a Product Requirements Document, authored as an Issue ([template](docs/templates/prd-template.md)). Defines problem, users, scope, non-goals, UX states, data model, API, risks, and acceptance criteria.
- **Task breakdown** — decompose the PRD into small, single-concern tasks. Draw the dependency graph and group tasks into **waves**: everything in a wave is independent and runs in parallel; waves run in order.
- **Issues** — each task becomes an Issue ([template](docs/templates/task-template.md)) linked to its PRD.
- **Implementation** — one agent or human per Issue, each isolated in its own git worktree on a feature branch.
- **Review / Merge** — small PR per Issue, CI gates, human approval, squash-merge, auto-deploy.
- **QA** — execute the test plan ([template](docs/templates/test-plan-template.md)); file follow-up Issues for findings.

Full operational detail: [`docs/process/spec-driven-workflow.md`](docs/process/spec-driven-workflow.md). Role definitions: [`docs/process/agent-roles.md`](docs/process/agent-roles.md).

---

## Definition of Done

A change is **Done** only when every box is checked:

- [ ] The linked Issue's acceptance criteria are all met.
- [ ] The change is a small, single-concern PR with a clear title and description.
- [ ] Every new or changed behavior is covered by a test that fails without the change.
- [ ] Lint, typecheck, and the full test suite pass in CI.
- [ ] All four+ UX states are handled where the change touches a UI surface (see below).
- [ ] No secrets, credentials, or tokens are present in the diff.
- [ ] Docs / templates / conventions updated if the change affects them.
- [ ] A human has reviewed and explicitly approved the merge.
- [ ] The PR closes its Issue (e.g. `Closes #123`).

---

## Testing pyramid (non-negotiable)

We test in proportion: many fast unit tests, fewer integration tests, a thin layer of end-to-end tests. The tools named below are **illustrations**, not requirements — use your stack's equivalents.

```
        ▲  fewer, slower, highest-confidence
       /E2E\        end-to-end: real user flows through a real build
      /------\      (e.g. a browser driver such as Playwright)
     /  integ \     integration: real collaborators / real dependencies
    /----------\    (e.g. a real DB or broker via testcontainers)
   /    unit     \  unit: pure logic, in isolation, milliseconds each
  /--------------\  the broad, fast base — most of your tests live here
```

- **Unit** — pure functions and isolated logic, no I/O, no network. Fast enough to run on every save. This is the widest layer.
- **Integration** — exercise real collaborators (database, queue, cache, external service) rather than mocks where it matters. Spinning real dependencies in ephemeral containers (e.g. **testcontainers**) keeps these honest and reproducible.
- **End-to-end** — a few high-value journeys through a real build, driven the way a user would drive them (e.g. a browser automation tool such as **Playwright**). Cover the critical paths, not every permutation.

Rules: a behavior change without a test does not merge. Don't mock what you can cheaply run for real. Keep E2E small and deterministic.

---

## UX states (non-negotiable)

Any view that loads or mutates data must render these states **distinctly** — never a blank screen, never a spinner that doubles as an error:

- **Loading** — work is in flight; show progress, not a frozen or empty UI.
- **Not connected / unauthorized** — the user isn't signed in, lacks access, or the session/connection is missing; prompt the correct next action.
- **Empty** — the request succeeded but there's nothing to show; explain what would populate it and offer the first action.
- **Error** — something failed; state what failed in plain language and offer a retry or recovery path.

The happy "loaded with data" state is necessary but not sufficient. A surface that only handles the happy path is not Done.

---

## Amending this Constitution

Change it the way we change anything else: open an Issue describing the *why*, open a small PR, get human approval, merge. The Constitution is itself spec-driven.
