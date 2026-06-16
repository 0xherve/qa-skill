# Test Cases Format

The test suite lives under `test-cases/`, one folder per module. Markdown is the
source of truth.

```
test-cases/
  <module>/
    README.md            ← module description + summary table
    TC01_<Action>.md     ← one test case per file
```

## Module folder

A **module** is a coherent area of the app (Documents, Organisations, Reviews).
Fix the canonical name on first `explore` and reuse it exactly — never mix spellings.

Modular numbering: each module numbers from TC01.

## README.md (per module)

Short description, then a summary table:

```markdown
# Documents

Upload, standardize, review, and publish policy documents.

| S.NO | TC Name | Description | Status |
|------|---------|-------------|--------|
| 1 | TC01_Document_Upload | Verify Platform Admin can upload a document. | Passed |
| 2 | TC02_Document_View | Verify uploaded documents appear in the Public Library. | Passed |
```

Keep in sync with TC files after every run.

## TC file template

```markdown
# TC01_Document_Upload

**Description:** Verify Platform Admin can upload a document through the complete workflow.
**Type:** Happy Path
**Preconditions:**
- Logged in as Platform Admin
- At least one category exists in the system
**Status:** Failed

## Steps

| Index | Action | Input Data | Expected Result | Result |
|-------|--------|------------|-----------------|--------|
| 1 | Navigate to Documents | — | Documents page displayed | As expected |
| 2 | Click Upload | — | Upload form displayed | As expected |
| 3 | Enter document title | Valid title | Title field populated | As expected |
| 4 | Choose category | A category | Category selected | Failed |
| 5 | Click Continue | — | Standardization step shown | To be tested |
| 6 | Confirm standardized content | — | Content displayed for review | To be tested |
| 7 | Click Submit | — | Success toast "Document Submitted Successfully." Redirected to Public Library. | To be tested |

## Remarks

Step 4: Category dropdown was empty — no categories available to select.
```

## Field rules

- **Description** — starts with "Verify"/"Check". One capability per TC. Business
  outcome, not clicks.
- **Type** — Happy Path, Negative, Edge Case, or Role-Based.
- **Preconditions** — bullet list when multiple. Always assume logged in — never
  list login as a step. Use a single line when there's only one precondition.
- **Status** — Passed · Failed · Uncertain · Skipped. Drafts start `Skipped`.
- **Action** — one action per row, verb first. Use the app's exact labels.
- **Input Data** — describe what to enter, not literal values. `—` if none.
- **Expected Result** — observable state, exact message text where visible. Describe
  *intended* behaviour, not just what the app happened to do. Never "it works."
- **Result** — `To be tested` on explore (draft). After a run: `As expected`,
  `Failed`, or `To be clarified`. Details go in Remarks, not in the table.
- **Remarks** — optional section. Only include when there's something worth noting
  (quirks, known issues, environment-specific behavior). Omit entirely if nothing
  to add.

## Naming

`TC[number]_[Module]_[Action]` — zero-padded number, singular PascalCase module,
PascalCase business action.
Good: `TC01_Document_Upload`. Bad: `TC5_Organisations_Form`.

## Confidence rubric

Every executed result carries a confidence level that maps to status:

| Confidence | What you observed | Default status |
|------------|-------------------|----------------|
| **High** | Expected result explicitly visible — exact toast, redirect, badge. No ambiguity. | Passed or Failed |
| **Moderate** | Result inferred from UI state but no definitive message confirmed it. | Uncertain |
| **Low** | No clear signal. Page changed but nothing confirmed success or failure. | Skipped |

Commit to Pass/Fail only at High confidence. Moderate defaults to Uncertain — only
promote to Pass/Fail when the evidence is strong but just short of explicit. Low means
leave it for the human to verify manually.
