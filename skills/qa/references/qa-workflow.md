# QA Workflow

The three modes and how they work.

## Scope

`scope` is `all` or one or more module names, comma-separated
(`/qa run documents,organisations`). Match to existing module folders under
`test-cases/`. If a named module doesn't exist yet, offer to `explore` it first.

## explore — discover & draft

1. Confirm app URL and scope. Human must be logged in.
2. Navigate every page, feature, form, and state in scope. Note inputs, outcomes,
   and edge cases. Don't interrupt mid-exploration.
3. Report a feature inventory grouped by module.
4. Draft test cases into `test-cases/<module>/` per `test-cases-format.md`.
   Every drafted TC starts `Skipped`. **Explore never executes a test.**
5. Update each module README summary table.

## run — execute

1. Confirm scope. Human must be logged in.
2. Execute each TC: follow its steps, adapt to UI drift, observe the outcome.
   Skip the un-doable. See `run-mechanics.md`.
3. Batch the whole suite — no per-TC confirmation. Write each result to the
   module JSON as you finish it.
4. Update current status on each TC file and module README.
5. Report: print the results table in the thread **and** save the run JSON.

## update — re-explore & reconcile

1. Re-explore the scope as in `explore`.
2. Diff against existing test cases:
   - **Renamed** labels/flows → update the steps, note the rename.
   - **Removed** features → retire affected TCs (mark clearly, don't delete).
   - **New** features → draft new TCs (`Skipped`), numbered next in that module.
3. Never silently rename or renumber an existing TC. If a feature's identity
   changed, record it as a rename. Keep modular numbering.
4. Report every change.
