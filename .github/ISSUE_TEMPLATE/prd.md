---
name: PRD
about: A Product Requirements Document — the what & why, authored as a living Issue.
title: "PRD: <short title>"
labels: [prd]
---

<!--
This mirrors docs/templates/prd-template.md. Define the WHAT and WHY, never the HOW.
Ready when every section is filled and the acceptance criteria are testable.
-->

## Problem

<!-- What's broken or missing, and why it matters now. Quantify the pain if you can. -->

## Users

<!-- Who is this for, their context, and what success looks like for them. -->

## Scope

<!-- What this WILL deliver. -->

-

## Non-goals

<!-- What this explicitly WILL NOT do. As important as Scope. -->

-

## UX states

<!-- For each affected surface: Loading / Not connected / Empty / Error / Loaded. -->

| Surface | Loading | Not connected | Empty | Error | Loaded |
|---------|---------|---------------|-------|-------|--------|
|         |         |               |       |       |        |

## Data model

<!-- Entities, fields, relationships, migrations. -->

## API / interfaces

<!-- Endpoints, events, functions, contracts — with input/output shapes. -->

| Interface | Input | Output | Notes |
|-----------|-------|--------|-------|
|           |       |        |       |

## Risks & open questions

-

## Acceptance criteria

<!-- Testable. Prefer Given / When / Then. The change is done when all are true. -->

- [ ] Given ..., when ..., then ...

---

*Next: the Orchestrator breaks this into task Issues grouped in waves.*
