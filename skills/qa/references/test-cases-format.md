# Test Cases Format (markdown)

The test suite is the source of truth. It lives under `test-cases/`, one folder per
module. Markdown, because humans read and edit it directly.

```
test-cases/
  <module>/
    README.md            ← module description + summary table (current status)
    TC01_<Action>.md     ← one test case
    TC02_<Action>.md
```

## Module folder

A **module** is a coherent area of the app — Documents, Organisations, Reviews,
Publishing, Users. On first `explore`, you fix the canonical name for each module
(folder name and the name used everywhere). Watch spelling: pick `Organisation` *or*
`Organization` once and never mix. On later runs, reuse the exact existing folder
names — never invent a variant.

Modular numbering: **each module numbers from TC01.** Documents has its own TC01,
Organisations has its own TC01. This way adding or removing a module never forces a
renumber.

## README.md (per module)

A short description of what the module does, then a summary table with current status.

```markdown
# Documents

Upload, standardize, review, version, and publish policy documents. Covers the public
library, drafts, comments, and the review workflow.

| S.NO | TC Name | Description | Status |
|------|---------|-------------|--------|
| 1 | TC01_Document_Upload | Verify Platform Admin can upload a document through the complete workflow. | Passed |
| 2 | TC02_Document_View_Uploaded | Verify uploaded documents appear in the Public Library with correct details. | Passed |
| 3 | TC06_Document_Save_Draft | Verify Platform Admin can save an in-progress upload as a draft. | Failed |
```

Keep the table in sync with the TC files after every run.

## TCxx_<Action>.md (per test case)

```markdown
# TC01_Document_Upload

**Description:** Verify Platform Admin can upload a document through the complete
workflow (upload, standardize, review, submit).
**Preconditions:** Logged in as Platform Admin.
**Status:** Passed

## Steps

| Index | Test Steps | Input | Expected Result |
|-------|-----------|-------|-----------------|
| 1 | 1. Navigate to Documents 2. Click Upload | N/A | Upload form displayed |
| 2 | 1. Enter document title 2. Choose category 3. Click Continue | Valid title, a category | Standardization step shown |
| 3 | 1. Confirm standardized content 2. Click Submit | N/A | Success toast "Document Submitted Successfully." Redirected to Public Library. Status shown as Submitted. |
```

### Field rules
- **Status** is the current status (see `status-confidence.md`). Drafts start `Skipped`.
- **Preconditions** state the entry condition. Always assume logged in — never list
  login as a step.

### Steps are guidance, not a rigid script
During a run you **follow** the steps, but the UI is the truth. If a label changed
("Upload" → "Add Document"), adapt, complete the test, and flag the drift in the report.
A changed label is not a failure on its own.

### Writing rules
- **Test Steps** — one action per numbered line, verb first, target only. Actions only;
  verifications belong in Expected Result. Use the app's exact labels. Skip login.
- **Input** — describe what to enter, not literal values (no real emails). "N/A" if none.
- **Expected Result** — noun-led, observable state. Exact message text where visible.
  Specific enough for a clean pass/fail. Never "it works". Describe *intended/correct*
  behaviour, not merely what the app happened to do — or you enshrine a bug as expected.
- All three columns: numbered lists when there are multiple items, plain text for one.
  "N/A" for empty.

### Indexing
- Same route + same flow = same index. Modals and dialogs are part of the same flow.
- No hard step limit per index; later indexes assume continuity from previous ones.

### Description (the one-liner)
Start with "Verify"/"Check". One capability per test case. Describe the business
outcome, not what the user clicks (no "clicks"/"enters"/"presses").
Good: `Verify organization members can be filtered by role and status.`

### Naming
`TC[number]_[Module]_[Action]` — zero-padded number, singular PascalCase module,
PascalCase business action (not a UI action).
Good: `TC10_Organization_Create`. Bad: `TC5_Organisations_Form`.

## Other formats

Markdown is primary. The same content can be expressed as Excel or JSON when a team
needs it — see `documenting-test-cases.md` and `navigating-sheets.md`. Keep markdown
the source of truth and export from it.
