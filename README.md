# Spec-Driven, Agent-First Project Template

A starter template for building software with autonomous coding agents **without giving up control.** It encodes a simple bet: you move fastest when you agree on the *what* and the *why* in writing first, then let agents (and humans) execute the *how* in parallel — behind gates that never let unreviewed code reach production.

This is a GitHub **template repository**. Click **Use this template**, then adapt the constitution and conventions to your stack. Everything here is intentionally framework-agnostic.

---

## Why spec-driven *and* agent-first

The usual framing pits velocity against control: ship fast and break things, or move carefully and crawl. That tradeoff is mostly an artifact of *where* the thinking happens. When intent lives only in someone's head, every handoff — to a teammate, to a future you, to an agent — leaks information, and you pay for it in rework and review friction. The fix is to push the expensive thinking to the front: decide the problem, the users, the scope, the non-goals, and the acceptance criteria **in writing**, before a single line of implementation. Once intent is explicit and agreed, execution becomes cheap to parallelize and cheap to verify.

Agents make that front-loading pay off enormously. A clear, self-contained spec is exactly the input an autonomous coding agent needs to do good work unsupervised — and exactly what lets you fan out five agents across five independent tasks at once. But agents also make discipline non-negotiable: more code produced per hour means more code that can quietly go wrong. So the same specs that brief the agents also become the contract you review against, and a fixed set of gates (tests, types, lint, human approval) stands between any generated diff and `main`. Velocity comes from parallelism and clear intent; control comes from gates that are never optional.

The last principle is that **specs are living GitHub Issues, not stale files in the repo.** A markdown doc in `/docs` rots the moment the code moves on. An Issue is a unit of work with an owner, a status, a thread of decisions, and a natural link to the PR that closes it. Authoring PRDs and tasks *as Issues* means the spec, the conversation, and the delivery all live in one auditable place — and it's what makes the "ship from your phone" loop below possible.

---

## The lifecycle

```
   IDEA
     │  a problem worth solving, in one or two sentences
     ▼
   PRD  ───────────────────────────────────────────────────  (a GitHub Issue)
     │  problem · users · scope · non-goals · UX states
     │  data model · API · risks · acceptance criteria
     ▼
   TASK BREAKDOWN
     │  decompose the PRD into small, single-concern tasks
     │  draw the dependency graph, then group into WAVES:
     │
     │      wave 1     ┌── task A ──┐      (no deps — run in parallel)
     │                 └── task B ──┘
     │                       │  (wave 2 depends on wave 1)
     │      wave 2          task C
     │                       │
     │      wave 3       ┌── task D ──┐    (parallel again)
     │                   └── task E ──┘
     ▼
   ISSUES  ────────────────────────────────────────────────  (one per task)
     │  each task becomes an Issue linked to the PRD
     ▼
   IMPLEMENTATION
     │  one agent (or human) per Issue, each in its own
     │  git worktree on a feature branch — isolated, parallel
     │  within a wave; sequential across waves
     ▼
   REVIEW / MERGE
     │  small PR per Issue → CI gates → explicit human approval
     │  → squash-merge → auto-deploy
     ▼
   QA
     │  run the test plan against the deployed change;
     │  file follow-up Issues for anything found
     ▼
   DONE
```

See [`docs/process/spec-driven-workflow.md`](docs/process/spec-driven-workflow.md) for the copy-pasteable playbook, and [`docs/process/agent-roles.md`](docs/process/agent-roles.md) for who does what.

---

## The "ship from your phone" loop

The point of encoding the process this way is that the whole delivery path can run from an Issue thread — including from your phone:

```
  you open or comment on an Issue   ──▶   a CI agent picks it up
            │                                      │
            │                                opens a draft PR with the change
            │                                      │
            │                              CI gates run (lint · types · tests)
            │                                      │
       you review the PR ◀───────────────────  gates green?
            │
       you approve + merge (the one human-only step)
            │
            ▼
       auto-deploy on merge to main
```

Mentioning the agent on an Issue (e.g. a comment like `@agent implement this`) triggers the workflow in [`.github/workflows/agent.yml`](.github/workflows/agent.yml). The agent reads the Issue (which *is* the spec), opens a PR, and [`.github/workflows/ci.yml`](.github/workflows/ci.yml) enforces the gates. The only step a human must perform is the merge — the system is designed so that no unreviewed, un-approved code can ever reach `main`.

---

## How to use this template

1. **Create your repo** from this template (**Use this template** ▸ *Create a new repository*), or `gh repo create <name> --template <owner>/spec-driven-starter`.
2. **Read [`CONSTITUTION.md`](CONSTITUTION.md).** It's the highest-level "how we work." Adjust the prime directives if you must, but treat the gates as non-negotiable.
3. **Customize [`CLAUDE.md`](CLAUDE.md)** (rename to `AGENTS.md` if you prefer) with your stack's coding conventions. This is the per-project brief every agent reads first.
4. **Wire CI.** Fill in the commented placeholders in [`.github/workflows/ci.yml`](.github/workflows/ci.yml) with your real lint / typecheck / test commands.
5. **Wire the agent loop.** In [`.github/workflows/agent.yml`](.github/workflows/agent.yml), point the placeholder step at your coding-agent action and add the required secret in your repo settings (never hardcode a token).
6. **Author your first PRD** as an Issue using the **PRD** issue template, break it into tasks using the **Task** template, and run the workflow.

### What's in the box

```
.
├── README.md                              this file
├── CONSTITUTION.md                        prime directives · DoD · testing & UX rules
├── CLAUDE.md                              per-project agent conventions (customize)
├── LICENSE                                MIT
├── docs/
│   ├── process/
│   │   ├── agent-roles.md                 the five roles + handoffs
│   │   └── spec-driven-workflow.md        the operational playbook
│   └── templates/
│       ├── prd-template.md
│       ├── task-template.md
│       └── test-plan-template.md
├── .claude/
│   └── skills/                            bundled lifecycle skills (specify · plan-waves · implement-feature · review-pr)
└── .github/
    ├── workflows/
    │   ├── ci.yml                         lint · typecheck · test gates
    │   └── agent.yml                      comment-an-agent → PR loop
    └── ISSUE_TEMPLATE/
        ├── prd.md
        └── task.md
```

The bundled `.claude/skills/` are copies from the canonical [`claude-code-skills`](https://github.com/nicoguedes/claude-code-skills) collection — pull updates from there.

---

## Principles in one breath

- Agree on the **what/why** in writing before the **how**.
- Specs are **living Issues**, not stale docs.
- **Never push to `main`.** Always a PR. Never merge without a human.
- **Small, single-concern PRs.** Every behavior change ships a test.
- **Parallel within a wave, sequential across waves.** Isolate work in worktrees.
- **Gates are never optional.** Velocity comes from clarity, not from skipping review.

License: [MIT](LICENSE).
