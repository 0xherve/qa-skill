# Run Mechanics

How a `run` actually behaves once it's underway. The goal: finish the whole suite in
one batch, never abort on a single bad test, and hand the human a clear report.

## Batch, don't babysit

Run every TC in scope without pausing for per-TC confirmation. The human reviews the
report at the end, not each step. Write each result to the run JSON as you finish it.

## Skipping — what you don't attempt

Mark `Skipped` and move on (no confirmation) when a test requires anything you can't do
through browser navigation alone:

- File uploads (documents, images, PDFs)
- Email verification or reading an inbox
- OTP or auth-code entry
- Credentials you weren't given
- Anything outside the browser

Record one line of why: `"Requires file upload — run manually."`

## UI drift — adapt and flag

Steps are guidance. If a label, position, or wording changed but the capability still
works, adapt, complete the test, and record the change in `drift`. A changed label is
not a failure. If the change is big enough that the test case is now wrong, flag it for
an `update`.

## What run writes — and what it doesn't

`run` updates the **current status** on each TC file and module README, and writes
results to the run JSON. It does **not** touch definitions — steps, descriptions, or
which test cases exist. Fixing those is `update`'s job.

## Uncovered features — flag, don't draft

If you come across a feature with no test case at all (not drift — genuinely
uncovered), name it in the report: "Uncovered: feature X — consider an `update`." Don't
draft a test case for it during a run. Surfacing it makes sure nothing slips past while
keeping `run` read-only on the suite.

## Failures — confirm, don't cry wolf

Only mark `Failed` when you're confident it's a genuine defect, not intended behaviour
(this protects the valid-bug-rate KPI). When unsure whether it's a real bug, mark
`Uncertain` and let the human judge. For a real failure, record: what you expected, what
happened, and at which step. Then continue to the next TC — one failure never aborts the
run.

## Mid-run problems

- **Not logged in / session expired** — stop and ask the human to log in. This is the
  one hard stop (you never handle auth). Resume from where you left off.
- **Page won't load / transient error** — retry once. If it still fails, `Skip` that TC
  with the reason and continue.
- **App is down entirely** — stop, and report what you completed plus that the app was
  unreachable. Don't fabricate results for the rest.

## Preconditions & test data

- Each TC sets up its own preconditions where it can. Don't assume one TC's leftover
  state for another — if a precondition can't be met, `Skip` with the reason.
- When you create data while testing (organisations, documents, members), make it
  identifiable — prefix with something like `QA_<date>` where the UI allows — so the
  team can tell test data apart. Don't delete data to "clean up"; deletion is itself
  risky and others may want to see it. List notable destructive actions you took in the
  report.

## Resuming

Because results are written incrementally, a run interrupted (context limit, crash, the
human stopping you) can be resumed: read the existing `<module>.json`, skip TCs already
recorded, and continue.
