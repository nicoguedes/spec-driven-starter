# Test plan: <feature / PRD title>

> A manual QA pass run against the **deployed** change, beyond what automated tests cover. Owned by [QA](../process/agent-roles.md#qa). File a follow-up Issue for every failure.

**PRD:** #<prd-number>
**Environment:** <where this was tested — e.g. staging URL, preview deploy>
**Tested by:** <name / agent>
**Date:** <yyyy-mm-dd>

## Preconditions

What must be true before testing (test accounts, seed data, feature flags, access).

- ...

## Test cases

| # | Scenario | Steps | Expected result | Result (pass/fail) | Issue |
|---|----------|-------|-----------------|--------------------|-------|
| 1 | Happy path | ... | ... | | |
| 2 | ... | ... | ... | | |

## UX states checklist

Confirm each surface renders every state distinctly (see the [Constitution](../../CONSTITUTION.md#ux-states-non-negotiable)):

- [ ] **Loading** shown distinctly (not a frozen/blank screen).
- [ ] **Not connected / unauthorized** prompts the correct next action.
- [ ] **Empty** explains what would populate it and offers a first action.
- [ ] **Error** states what failed in plain language and offers retry/recovery.
- [ ] **Loaded** renders correctly with real data.

## Acceptance criteria verification

Walk the PRD's acceptance criteria one by one against the live change.

- [ ] AC 1: ...
- [ ] AC 2: ...

## Result

- **Outcome:** PASS / FAIL
- **Defects filed:** #..., #...
- **Notes:** ...
