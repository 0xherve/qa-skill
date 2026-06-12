# Browser Actions (MCP-agnostic)

You drive a browser through whatever MCP server is configured — Playwright, Chrome
DevTools, or another. The skill never hard-codes tool names. Think in **capabilities**;
map them to whatever tools the configured server exposes.

## Capabilities you rely on

| Capability | What you use it for |
|------------|---------------------|
| **Navigate** | Go to a URL or follow in-app links. |
| **Read page** | Get the current page's text/structure to know what's there. |
| **Click** | Buttons, tabs, links, menu items — by their visible label. |
| **Type** | Fill fields with described input (never real credentials/emails). |
| **Select / set** | Dropdowns, toggles, checkboxes, radio buttons. |
| **Screenshot** | Capture state — especially on Failed/Uncertain for evidence. |
| **Wait / observe** | Let async UI settle before reading the result. |

## Rules

- **Read the live UI as truth.** Use exact on-screen labels and message text; quote
  them verbatim in expected/actual results.
- **Never handle login.** The human authenticates first; you take over the session. If
  not logged in, stop and ask (see SKILL.md → Auth).
- **Observe before you judge.** Wait for toasts, redirects, and async updates to appear
  before deciding pass/fail. A premature read produces a false result.
- **Derive results only from what's observable.** If nothing on screen confirms the
  outcome, your confidence is Low — don't invent a result.

## When the configured server changes

Nothing in the suite or the workflow references a specific tool. If the QA engineer
swaps browser MCPs, only the mapping in this file's mental model changes — re-map the
capabilities above to the new tool set and continue.
