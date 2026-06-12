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
- Before editing, screenshot to confirm you're on the right tab.
- Check where the data ends before starting — don't assume row numbers.

### Name Box navigation

The Name Box (top-left, shows current cell ref) is the only reliable way to jump:

1. Triple-click the Name Box to select its contents.
2. Type the target cell reference (e.g. `A87`).
3. Press **Enter** to jump.

Never click near a cell to navigate — a wrong click can rename the sheet or land in
the wrong cell.

### Dropdown cells

Some columns (e.g. Status) use data-validation dropdowns:

1. Click the small arrow at the **right edge** of the cell to open the list.
2. Wait for the dropdown to render — option y-coordinates shift depending on the
   cell's position on screen.
3. Click the option text carefully. If you mis-click, clear the cell and retry via
   the dropdown — don't type freehand into a dropdown cell.

## Data entry

### Preparation

Draft all cell values locally (in a variable or scratch note) before touching the
sheet. Composing text while navigating causes errors and misalignment.

### Row-by-row fill order

Fill one complete row before moving to the next — never jump between rows.

When you have all values for a row ready, enter them as a single batch using **Tab**
to advance between cells. The flow:

1. Navigate to column A of the target row (Name Box).
2. Type the value for column A, press **Tab** → moves to column B.
3. Type the value for column B, press **Tab** → moves to column C.
4. Continue through every column (A → B → C → D → E → F → G → H), pressing **Tab**
   after each. Never click to move between cells mid-row — Tab keeps you on track.
5. Press **Enter** after the last column to commit and move down to the next row.
6. **Verify checkpoint**: screenshot the completed row to confirm alignment before
   starting the next one.

### Cell content rules

- Type `N/A` for empty columns, never skip.
- **Enter** finishes a row and moves to the next.
- For an in-cell line break, use **Ctrl+Enter** — never `\n` in typed text, which
  creates a new row instead. Ctrl+Enter occasionally fails silently and concatenates
  — verify after. When in doubt, use semicolons to separate multi-step content
  instead of line breaks.
- Don't put `\t` or `\n` in typed text; they don't behave as expected.
- For multi-step cells (Test Steps, Input, Expected Result), number each item and
  put each on its own line using **Ctrl+Enter** between them.
- Tab commits and moves right (same row).

## Fixing errors

- To fix a cell: Name Box → jump to it → read it → clear → retype.
- Never use bulk undo (Ctrl+Z repeated) — it wipes good edits along with bad ones.
  Fix individual cells directly.
- Avoid cut/paste between cells — clear and retype instead to reduce misalignment
  risk.

## Verification pass

After completing all data entry (or a logical batch of rows), do a full verification:

1. Navigate to the first row you edited (Name Box).
2. Screenshot the visible rows — cover every row you touched, scrolling if needed.
3. Compare each cell against your drafted values. Check:
   - Column alignment — did any content spill into the wrong column or row?
   - Dropdown values — correct option selected (Passed vs Failed, etc.)?
   - Multi-line content — did `\n` accidents create extra rows?
   - No blank cells where `N/A` should be.
4. Fix any mismatches immediately (Name Box → clear → retype).
5. Screenshot again after fixes to confirm the correction landed.

This pass is mandatory, not optional. Misalignment errors are invisible during entry
and compound across rows — catching them early is far cheaper than unwinding later.

## Reporting into the sheet

- Match the column order exactly — the team pastes/edits directly.
- For run results, fill Actual Result and Status for every executed step; put failure
  detail in Review Description and observations in Remarks.
- Keep the markdown suite and JSON runs as the primary record; the sheet is a shared
  view exported from them.
