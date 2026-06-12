# QA Workflow

Fuller mechanics of the three modes. SKILL.md has the short version; this is the detail.

## Scope

`scope` is `all` or one or more module names, comma-separated
(`/qa run documents,organisations`). Match scope to existing module folders under
`test-cases/`, tolerant of minor spelling differences. If a named module doesn't exist
yet, offer to `explore` it first. `all` means every module in scope for that app.

## explore — discover & draft

Use when test cases don't exist yet, or to cover new ground.

1. **Confirm** app URL and scope. Require the human logged in.
2. **Navigate** every page, feature, form, button, and state in scope. Note inputs each
   feature accepts, outcomes it can produce, and edge cases visible in the UI (empty
   states, validations, error messages, limits). Don't interrupt mid-exploration.
3. **Report the inventory** — features grouped by module, in plain language.
4. **Draft test cases** into `test-cases/<module>/`, following
   `test-cases-format.md`. One capability per test case. Expected results describe
   *intended/correct* behaviour, not merely whatever the app happened to do — otherwise
   a bug gets enshrined as "expected". Every drafted TC starts `Skipped` (to be done).
   **Explore never executes a test.**
5. **Update** each module README summary table.

## run — execute

Use when test cases exist and you want results.

1. **Confirm** scope. Require login.
2. **Execute** each TC: follow its steps as guidance, adapt to UI drift, observe the
   real outcome, set status + confidence per `status-confidence.md`. Skip the
   un-doable. See `run-mechanics.md`.
3. **Batch** the whole suite — no per-TC confirmation. Write each result to the module
   JSON as you finish.
4. **Update** current status on each TC file and module README.
5. **Report**: print the results table in the thread and save the run JSON
   (`runs-format.md`).

## update — re-explore & reconcile

Use when the app changed and the suite needs to catch up.

1. **Re-explore** the scope, as in `explore`.
2. **Diff** against the existing suite:
   - **Renamed** labels/flows → update the steps; note the rename.
   - **Removed** features → retire the affected TCs (mark clearly; don't delete history).
   - **New** features → draft new TCs (status `Skipped`), numbered next in that module.
3. **Apply and flag.** Make the changes; never silently rename or renumber an existing
   TC. If a feature's identity genuinely changed, record it as a rename rather than
   mutating in place. Keep modular numbering.
4. **Report** every change in the run report's `changes` list.

## Always

- The human logs in; you never handle credentials. Not logged in → stop and ask.
- Plain language, evidence over claims, honest uncertainty (`voice.md`).
- You flag possible bugs; the human decides if they're real.
