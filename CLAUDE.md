# Agent conventions

> **Customize this file per project.** This is the per-project brief every coding
> agent (Claude, or any other) reads before touching code. The example below is
> generic — replace it with your real stack, commands, and house style. Rename to
> `AGENTS.md` if your tooling prefers that name.

## Read first

1. [`CONSTITUTION.md`](CONSTITUTION.md) — the non-negotiable rules. They override anything here.
2. The Issue you're working on — it *is* the spec. Build exactly what it specifies; nothing more.

## Golden rules

- **Never push to `main`.** Work on `feat/<slug>`, open a PR, let a human merge.
- **One concern per PR.** If scope grows, file a new Issue instead of expanding the diff.
- **Every behavior change ships a test** that fails before your change and passes after.
- **No secrets in the repo, ever** — not in code, config, or CI. Use the secret manager.
- **Handle all UX states** (loading / not-connected / empty / error) on any surface you touch.

## Project setup

<!-- TODO: replace with the real commands for this repo. -->

```bash
# install:    <e.g. npm ci  |  pip install -e ".[dev]"  |  go mod download>
# run:        <e.g. npm run dev>
# lint:       <e.g. npm run lint>
# typecheck:  <e.g. npm run typecheck>
# test:       <e.g. npm test>
```

These must match the gates in [`.github/workflows/ci.yml`](.github/workflows/ci.yml).

## House style

<!-- TODO: pin your conventions so agents are consistent. Examples: -->

- Language / framework / version: `<...>`
- Formatting: `<formatter + config>` — run it before committing.
- Naming: `<conventions>`.
- Error handling: `<pattern — e.g. typed errors, no silent catches>`.
- Tests live in `<location>`; name them `<pattern>`.
- Don't introduce a new dependency without noting why in the PR.

## Directory map

<!-- TODO: a quick map so agents know where things go. -->

```
src/        <application code>
tests/      <tests>
docs/       process + templates (see docs/process/spec-driven-workflow.md)
```

## Commit & PR conventions

- Commit messages: `<type>: <summary>` (e.g. `feat:`, `fix:`, `refactor:`).
- PR body must link its Issue with `Closes #<n>` and summarize *what* and *how it's tested*.
