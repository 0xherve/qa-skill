# QA Workflow

The three modes and how they work.

## Scope

`scope` is `all` or one or more module names, comma-separated
(`/qa run documents,organisations`). Match to existing module folders under
`test-cases/`. If a named module doesn't exist yet, offer to `explore` it first.

## explore — discover & draft

1. Confirm app URL and scope. Human must be logged in.
2. **Read the BRD** for each in-scope module (`requirements/<module>/BRD.md`).
   If the BRD is missing, ask the engineer where to find it before continuing.
3. **Read the PD** if available (`requirements/<module>/PD.md`) for UI flow detail.
4. Navigate every page, feature, form, and state in scope. Note inputs, outcomes,
   and edge cases. Don't interrupt mid-exploration.
5. Report a feature inventory grouped by module.
6. Draft test cases into `test-cases/<module>/` per `test-cases-format.md`:
   - One TC per functional requirement (FR) in the BRD.
   - Steps cover the FR's acceptance criteria (ACs).
   - Use PD workflows for step detail; use app exploration for actual UI labels.
   - Each TC gets a `Source` field pointing to the BRD and FR.
   - If the app contradicts the BRD, write expected results per the BRD and flag
     the discrepancy in Remarks.
   Every drafted TC starts `Skipped`. **Explore never executes a test.**
7. Update each module README with a requirements reference and summary table.

## run — execute

1. Confirm scope. Human must be logged in.
2. **Read the BRD** for each in-scope module. The BRD is always consulted during run.
3. Execute each TC: follow its steps, adapt to UI drift, observe the outcome.
   Skip the un-doable. See `run-mechanics.md`.
4. **Validate against BRD:** cross-check observed results against BRD acceptance
   criteria. If the app passes the TC steps but violates a BRD criterion, flag it.
5. Batch the whole suite — no per-TC confirmation. Write each result to the
   module JSON as you finish it.
6. Update current status on each TC file and module README.
7. **Check coverage:** after the run, compare BRD acceptance criteria against
   existing TCs. Report any untested ACs as coverage gaps in the run report.
   Do not auto-draft TCs — just report the gaps.
8. Report: print the results table in the thread **and** save the run JSON.

## update — re-explore & reconcile

1. **Read the BRD** for each in-scope module. The BRD drives what TCs should exist.
2. Re-explore the app as in `explore` (but do **not** read the PD — it may be outdated).
3. Diff against existing test cases, using the BRD as the reference:
   - **New FRs in BRD** → draft new TCs (`Skipped`), numbered next in that module.
   - **Removed FRs from BRD** → retire affected TCs (mark clearly, don't delete).
   - **Changed ACs** → update TC steps to match current BRD criteria.
   - **Renamed** labels/flows in the app → update the steps, note the rename.
   - **App contradicts BRD** → update expected results per BRD, flag discrepancy.
4. Never silently rename or renumber an existing TC. If a feature's identity
   changed, record it as a rename. Keep modular numbering.
5. Report every change.
