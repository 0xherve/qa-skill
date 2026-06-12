# Statuses & Confidence

## The four statuses

| Status | Meaning |
|--------|---------|
| **Passed** | You verified the test case works. Clear, observable evidence matched the expected result. |
| **Failed** | You confirmed something is broken. You are sure it's a genuine defect, not intended behaviour. |
| **Uncertain** | You ran it and got a result, but can't confidently call it pass or fail — ambiguous expected outcome, partial evidence, or behaviour you can't interpret. |
| **Skipped** | You couldn't attempt it (upload, email, OTP, missing credentials, not reachable by browser) **or** you attempted it but had too little signal to claim anything. |

A freshly drafted test case (from `explore`) is **Skipped** — it hasn't been run yet.

## Confidence

Every executed result carries a confidence read: **High · Moderate · Low**. Confidence
is your honest sense of how strong the evidence was, and it maps to status:

```
HIGH       → Passed or Failed (definitive)
MODERATE   → Uncertain by default
             → Passed/Failed only when it's "almost high" — strong evidence,
               just not an exact, explicit confirmation
LOW        → Skipped (leave it for the human to run manually)
```

The rule of thumb: **commit to Pass/Fail only when you're sure.** Route doubt to
Uncertain. Route "couldn't really tell" to Skipped.

## Confidence rubric — what drives the read

| Read | What you observed |
|------|-------------------|
| **High** | The expected result was explicitly visible — exact success/error toast text, a redirect to the expected page, a status badge or count that matched. No ambiguity. |
| **Moderate** | The result was inferred from UI state, but no definitive message confirmed it. The app *looks* right but didn't say so. |
| **Low** | No clear signal. The page changed but nothing observable confirmed success or failure, or you couldn't reach the state at all. |

## Why this matters

The **valid-bug rate** — how many of your flagged bugs turn out to be real — is what
makes QA output trustworthy. A `Failed` you weren't sure about, that turns out to be
intended behaviour, erodes it. So: when in doubt, it's **Uncertain**, and the human
decides. Never inflate a shaky observation into a Failed to look thorough.

## How statuses land in artifacts

- The TC file (`test-cases/<module>/TCxx.md`) holds the **current** status only.
- The module README table reflects the same current status.
- The run JSON holds the status **for that run**, plus confidence and remarks — that's
  the history.
