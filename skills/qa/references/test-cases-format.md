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
**Preconditions:** Logged in as Platform Admin.
**Status:** Passed

## Steps

| Index | Test Steps | Input | Expected Result |
|-------|-----------|-------|-----------------|
| 1 | 1. Navigate to Documents 2. Click Upload | N/A | Upload form displayed |
| 2 | 1. Enter document title 2. Choose category | Valid title, a category | Standardization step shown |
| 3 | 1. Confirm content 2. Click Submit | N/A | Success toast "Document Submitted Successfully." |
```

## Writing rules

- **Test Steps** — verb first, one action per line. Use the app's exact labels. No login steps.
- **Input** — describe what to enter, not literal values. `N/A` if none.
- **Expected Result** — observable state, exact message text where visible. Describe
  *intended* behaviour, not just what the app happened to do. Never "it works."
- **Description** — starts with "Verify"/"Check". One capability per TC. Business
  outcome, not clicks.
- **Naming** — `TC[number]_[Module]_[Action]`, zero-padded, PascalCase.
- **Indexing** — same route + same flow = same index. Modals are part of the same flow.
- Empty cells get `N/A`.

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
