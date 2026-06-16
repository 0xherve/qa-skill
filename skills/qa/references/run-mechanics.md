# Run Mechanics

How a `run` behaves once underway. Finish the whole suite in one batch, never abort
on a single bad test, hand the human a clear report.

## Skipping

Mark `Skipped` and move on when a test requires anything outside browser navigation:
file uploads, email verification, OTP, credentials you weren't given. Record why in
one line.

## UI drift

If a label or wording changed but the capability still works, adapt, complete the
test, and record the change in `drift`. A changed label is not a failure. If the
change is big enough that the TC is now wrong, flag it for an `update`.

## Mid-run problems

- **Not logged in / session expired** — stop and ask the human to log in. Resume
  from where you left off.
- **Page won't load** — follow the Page Load Protocol in `browser-actions.md`. Only
  after multiple patient attempts, `Skip` that TC with the reason and continue.
- **App is down entirely** — stop, report what you completed, note the app was
  unreachable. Don't fabricate results.

## Test data

- Each TC sets up its own preconditions. Don't rely on leftover state from another TC.
- Prefix created data with `QA_<date>` where possible. Don't delete data to clean up.
- List notable destructive actions in the report.

## Resuming

Results are written incrementally. An interrupted run can be resumed: read the existing
module JSON, skip TCs already recorded, continue.

## What run writes

`run` updates current status on TC files and module READMEs, and writes to run JSON.
It does **not** touch definitions — that's `update`'s job.

## Uncovered features

If you find a feature with no TC at all, name it in the report. Don't draft a TC
during a run.
