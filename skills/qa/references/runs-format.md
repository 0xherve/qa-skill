# Runs Format (JSON)

Run history is JSON — machine-queryable, so changes between runs are easy to diff and
chart. The test-case markdown holds *current* status; the runs hold *what happened when*.

```
runs/
  YYYY-MM-DD_HHMM/         ← one folder per run (time included so same-day runs don't collide)
    report.json            ← run summary
    <module>.json          ← per-TC results for that module
```

## Two outputs, always

At the end of a run you produce **both**:
1. The results **table printed in the thread** (for the human to read now).
2. The run **JSON saved** to disk (for history and querying).

Write each TC result to its module JSON **as you finish it**, not all at the end, so an
interrupted run keeps its progress.

## `<module>.json`

```json
{
  "module": "documents",
  "run": "2026-06-12_1430",
  "app": "OpenDoc",
  "url": "https://staging.opendoc.example",
  "results": [
    {
      "tc": "TC01_Document_Upload",
      "status": "Passed",
      "confidence": "High",
      "remarks": "",
      "drift": ""
    },
    {
      "tc": "TC06_Document_Save_Draft",
      "status": "Failed",
      "confidence": "High",
      "remarks": "Save Draft button unresponsive; no draft created, no error shown.",
      "drift": ""
    },
    {
      "tc": "TC15_Organizations_Invite_Admin",
      "status": "Uncertain",
      "confidence": "Moderate",
      "remarks": "Invite submitted but no confirmation message; can't tell if it sent.",
      "drift": ""
    },
    {
      "tc": "TC24_Organizations_Logo_Upload",
      "status": "Skipped",
      "confidence": null,
      "remarks": "Requires file upload — run manually.",
      "drift": ""
    }
  ]
}
```

- `status` — Passed · Failed · Uncertain · Skipped.
- `confidence` — High · Moderate · null (null when Skipped/not attempted).
- `remarks` — for Failed/Uncertain: what was expected, what happened, where. Blank when
  Passed clean.
- `drift` — note any UI change you adapted to (e.g. "'Upload' button now 'Add Document'").

## `report.json`

```json
{
  "run": "2026-06-12_1430",
  "app": "OpenDoc",
  "mode": "run",
  "scope": ["documents", "organisations"],
  "totals": { "passed": 10, "failed": 3, "uncertain": 1, "skipped": 2 },
  "failed": ["TC06_Document_Save_Draft", "TC10_Document_Schedule_Review"],
  "uncertain": ["TC15_Organizations_Invite_Admin"],
  "skipped": ["TC24_Organizations_Logo_Upload"],
  "changes": [],
  "notes": "Documents upload-from-draft flow broken on staging."
}
```

- `changes` — only for `update` runs: renames, retired TCs, new TCs. Flag every change here.
- `notes` — short, plain-language summary. Honest, no padding.

## The thread table

Mirror the run in a scannable table, then a short summary:

```
| TC | Module | Status | Confidence | Remarks |
|----|--------|--------|------------|---------|
| TC01_Document_Upload | Documents | Passed | High | |
| TC06_Document_Save_Draft | Documents | Failed | High | Save Draft button unresponsive |
| TC15_Organizations_Invite_Admin | Organisations | Uncertain | Moderate | No confirmation after invite |
| TC24_Organizations_Logo_Upload | Organisations | Skipped | — | Requires file upload |

10 passed · 3 failed · 1 uncertain · 2 skipped.
Needs your eyes: TC15 (uncertain), and the 2 skipped (manual).
```
