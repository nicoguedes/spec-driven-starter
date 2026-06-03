# PRD: <title>

> A Product Requirements Document. Author this **as a GitHub Issue** (label: `prd`). It defines the *what* and *why* — never the *how*. It's ready when every section below is filled and the acceptance criteria are testable.

## Problem

What's broken or missing, and why it matters now. One or two paragraphs. Quantify the pain if you can.

## Users

Who is this for? Their context, what they're trying to accomplish, and what success looks like *for them*.

## Scope

What this change *will* deliver. Bullet the concrete capabilities.

- ...

## Non-goals

What this change explicitly will **not** do. This section prevents scope creep and is as important as Scope.

- ...

## UX states

For each affected surface, define how it behaves in every state (see the [Constitution](../../CONSTITUTION.md#ux-states-non-negotiable)):

| Surface | Loading | Not connected / unauthorized | Empty | Error | Loaded |
|---------|---------|------------------------------|-------|-------|--------|
| <view> | ... | ... | ... | ... | ... |

## Data model

The entities, fields, and relationships this introduces or changes. Note new persistence, migrations, or schema impacts.

## API / interfaces

The endpoints, events, functions, or contracts this adds or changes. Include shapes for requests/responses or inputs/outputs.

| Interface | Input | Output | Notes |
|-----------|-------|--------|-------|
| ... | ... | ... | ... |

## Risks & open questions

What could go wrong, what's uncertain, what needs a decision before or during implementation.

- ...

## Acceptance criteria

Testable, unambiguous statements. The change is done when all of these are true. Prefer `Given / When / Then`.

- [ ] Given ..., when ..., then ...
- [ ] ...

---

*Next step: the Orchestrator breaks this PRD into tasks grouped in [waves](../process/spec-driven-workflow.md#2-task-breakdown-into-waves-orchestrator), each authored with the [task template](task-template.md).*
