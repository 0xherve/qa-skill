# Excel — Reporting & Navigation

Markdown is the source of truth. Use Excel when the team works from a shared sheet:
to report test cases or run results into it, you need the exact column schema and the
mechanics of entering data through the browser.

## Column schemas

### Test cases sheet (8 columns)

| # | Column | Rule |
|---|--------|------|
| 1 | S. NO | Sequential integer per module (modular numbering — each module from 1). |
| 2 | Test Case Status | Passed · Failed · Uncertain · Skipped. (Drafts: Skipped / to be done.) |
| 3 | Test Case Name | `TC##_Module_Action` — see `test-cases-format.md` naming. |
| 4 | Test Case Description | One sentence, starts with "Verify"/"Check", business outcome. |
| 5 | Notes | Blank unless something is ambiguous or worth flagging. |
| 6 | Owner | Default `QA Team`; ask if unknown. |
| 7 | Product | App name observed during exploration (e.g. `OpenDoc`). |
| 8 | Method | Always `Manual`. |

### Run steps sheet (10 columns)

| # | Column | Rule |
|---|--------|------|
| 1 | Test Case | Exact TC name from the test-case sheet. |
| 2 | Index | Sequential integer per test case, from 1. Same route + flow = same index. |
| 3 | Test Steps | One action per numbered line, verb first. Actions only. |
| 4 | Input | What to enter, not literal values. `N/A` if none. |
| 5 | Expected Result | Noun-led, observable state; exact message text where visible. |
| 6 | Actual Result | `As Expected` if matched; otherwise describe the deviation specifically. |
| 7 | Status | Passed · Failed · Uncertain · Skipped. |
| 8 | Review Description | Why it failed / needs review. Blank if Passed. |
| 9 | Remarks | Defects, gaps, UX notes. Blank if none. |
| 10 | Jam Link | Paste if the human provides one, else blank. |

Empty cells get `N/A`, never skipped. Use the same status enum as the rest of the skill;
if migrating an old sheet that used "To be clarified", map it to `Uncertain`.

## Navigation (entering data through the browser)

- Check whether the sheet is already open before navigating to it.
- Use the **Name Box** to jump to a cell. Don't eyeball-click near it — a wrong click
  can rename the file.
- Before editing, screenshot to confirm you're on the right tab.
- Check where the data ends before starting — don't assume row numbers.

## Data entry

- One batched action per row.
- Go left to right (A → B → C → …) with **Tab** between cells.
- Type `N/A` for empty columns, never skip.
- **Enter** finishes a row and moves to the next.
- For an in-cell line break, use **Ctrl+Enter** (it occasionally fails silently and
  concatenates — verify after).
- Don't put `\t` or `\n` in typed text; they don't behave as expected.

## Fixing errors

- Verify after the full entry, not cell by cell.
- To fix a cell: Name Box → select it → read it → clear → retype.

## Reporting into the sheet

- Match the column order exactly — the team pastes/edits directly.
- For run results, fill Actual Result and Status for every executed step; put failure
  detail in Review Description and observations in Remarks.
- Keep the markdown suite and JSON runs as the primary record; the sheet is a shared
  view exported from them.
