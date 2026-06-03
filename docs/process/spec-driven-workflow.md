# Spec-driven workflow — the playbook

This is the operational, copy-pasteable version of the [lifecycle](../../CONSTITUTION.md#the-spec-driven-lifecycle). Roles referenced here are defined in [`agent-roles.md`](agent-roles.md). Commands use the GitHub CLI (`gh`) and `git`; substitute placeholders like `<slug>`, `<n>`, and `feat/...` for real values.

```
IDEA → PRD → TASKS (waves) → ISSUES → IMPLEMENT → REVIEW/MERGE → QA → DONE
```

---

## 0. Idea

Write the problem in one or two sentences. If it's worth doing, it's worth a PRD. Don't start branching yet.

## 1. PRD (Product Manager)

Author the PRD **as an Issue** so it's a living document with a thread, not a file that rots. Use the **PRD** issue template (it mirrors [`prd-template.md`](../templates/prd-template.md)).

```bash
gh issue create \
  --title "PRD: <short title>" \
  --label prd \
  --body-file docs/templates/prd-template.md   # then fill it in, or use the PRD issue form
```

A PRD is ready when it has: problem, users, scope, **non-goals**, the required UX states, data model, API surface, risks, and **testable acceptance criteria**. The PM signs off before breakdown begins.

## 2. Task breakdown into waves (Orchestrator)

Decompose the PRD into small, single-concern tasks. Draw the dependency graph, then group tasks into **waves**.

> **The wave rule:** tasks are **parallel *within* a wave** (no task in a wave depends on another in the same wave) and **sequential *across* waves** (a wave starts only once the previous wave is merged).

```
wave 1   [ task A ] [ task B ]        ← independent, run together
wave 2   [ task C ]                   ← depends on A and B
wave 3   [ task D ] [ task E ]        ← independent again, run together
```

Keep tasks small enough that one agent can finish one in a single focused session and the resulting PR is reviewable in a few minutes.

## 3. Issues (Orchestrator)

Turn each task into an Issue using the **Task** template (mirrors [`task-template.md`](../templates/task-template.md)). Record dependencies and the wave in the body, and link back to the PRD.

```bash
gh issue create \
  --title "[wave 1] <task title>" \
  --label task \
  --body "Part of PRD #<prd-number>. Wave 1. Depends on: none.

  See acceptance criteria below."
```

## 4. Implementation (Developer) — one worktree per task

Each task is built in **its own git worktree** on its own feature branch. Worktrees give every agent (or human) an isolated checkout against the same repo, so parallel work in a wave never collides on the working tree.

```bash
# from the repo root, one worktree per in-flight task
git worktree add ../wt-<slug> -b feat/<slug>
cd ../wt-<slug>

# ... agent or human implements the task + tests against the Issue's acceptance criteria ...

git add -A
git commit -m "feat: <what changed> (Closes #<n>)"
git push -u origin feat/<slug>
```

Open a small, single-concern PR that closes the Issue:

```bash
gh pr create \
  --title "feat: <task title>" \
  --body "Closes #<n>. Part of PRD #<prd-number>.

  ## What
  <one-paragraph summary>

  ## Tests
  <what proves it works>"
```

When a wave's worktrees are merged, remove them and start the next wave:

```bash
git worktree remove ../wt-<slug>
git branch -d feat/<slug>
```

### Or: the "ship from your phone" agent loop

Instead of running the above by hand, comment on the task Issue to dispatch the coding agent wired up in [`../../.github/workflows/agent.yml`](../../.github/workflows/agent.yml):

```
@agent implement this
```

The agent reads the Issue (the spec), opens a draft PR, and [`ci.yml`](../../.github/workflows/ci.yml) runs the gates. You review and merge from anywhere.

## 5. Review / merge (PR Reviewer / Merger)

The gate. CI must be green; a human must approve; the merge is the one irreducible human step.

```bash
gh pr checks <pr-number>     # confirm lint · typecheck · tests are green
gh pr review <pr-number> --approve
gh pr merge  <pr-number> --squash --delete-branch   # squash-merge → auto-deploy
```

Reject anything that isn't single-concern, lacks tests for new behavior, or fails the [Definition of Done](../../CONSTITUTION.md#definition-of-done).

## 6. QA (QA)

Against the deployed change, run the manual [test plan](../templates/test-plan-template.md). Probe the four UX states and the PRD's acceptance criteria in the real environment. File a follow-up Issue for every defect — don't patch out-of-band; let it re-enter the pipeline at the Orchestrator.

```bash
gh issue create --title "QA: <defect>" --label bug \
  --body "Found during QA of #<n>. Steps to reproduce: ..."
```

## 7. Done

A change is Done when its acceptance criteria hold, QA passes, and the PM signs off. The Issue is closed by its merged PR; the PRD Issue closes when all its tasks are Done.

---

## At a glance

| Step | Role | Artifact | Command |
|------|------|----------|---------|
| Idea | anyone | one-liner | — |
| PRD | Product Manager | PRD Issue | `gh issue create --label prd` |
| Breakdown | Orchestrator | waves + dep graph | — |
| Issues | Orchestrator | task Issues | `gh issue create --label task` |
| Implement | Developer | PR in a worktree | `git worktree add` · `gh pr create` |
| Review/Merge | PR Reviewer/Merger | squash-merge | `gh pr merge --squash` |
| QA | QA | test-plan result + Issues | `gh issue create --label bug` |
| Done | Product Manager | sign-off | close Issue / PRD |
