# Requirements Format

The `requirements/` folder is the source of truth for what the skill tests. It lives
alongside `test-cases/` and `runs/` in the workspace root.

```
requirements/
  <module>/
    BRD.md             ← mandatory — business requirements document
    PD.md              ← optional — product documentation
```

## BRD (Business Requirements Document)

The BRD defines **what** must be tested. It contains:

- **Functional Requirements (FRs)** — each FR describes a capability the app must have.
- **Acceptance Criteria (ACs)** — each FR has ACs that define pass/fail conditions.

The skill maps BRD content to test cases:
- One TC per FR (or a few per FR when multiple types apply: happy path, negative, edge case).
- Steps within each TC cover the FR's acceptance criteria.
- Each TC's `Source` field references the BRD file and FR ID.

## PD (Product Documentation)

The PD describes **how** the product implements the requirements — screens, flows,
user interactions. It is:

- **Optional** — the skill works without it.
- **Referenced only during explore** — for UI flow detail when drafting test steps.
- **Never trusted over the BRD** — PDs may be outdated. If PD and BRD conflict, the
  BRD wins.

## Discrepancy handling

When the app, PD, or existing TCs contradict the BRD:

1. The **BRD is correct** — write expected results per the BRD.
2. **Flag the discrepancy** in TC Remarks (during explore/update) or run results
   (during run) so the engineer can review.
3. **Never silently adopt** app behavior over BRD criteria.

## Missing requirements

- **No BRD for a module** — ask the engineer where to find it before continuing.
- **No PD for a module** — proceed using BRD + app exploration only.
- **No requirements folder at all** — ask the engineer where the requirements files are.
