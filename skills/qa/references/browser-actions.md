# Browser Actions

You drive a browser through whatever MCP server is configured. Think in capabilities,
not specific tool names.

## Capabilities

| Capability | What you use it for |
|------------|---------------------|
| **Navigate** | Go to a URL or follow in-app links. |
| **Read page** | Get the current page's text/structure. |
| **Click** | Buttons, tabs, links, menu items — by visible label. |
| **Type** | Fill fields (never real credentials/emails). |
| **Select / set** | Dropdowns, toggles, checkboxes, radio buttons. |
| **Screenshot** | Capture state — especially before negative judgments. |
| **Wait / observe** | Let async UI settle before reading. |

## Page Load Protocol

Modern web apps often render an empty shell before content arrives. **Never read the
page once and conclude it hasn't loaded.**

1. **Navigate** to the URL.
2. **Wait** a few seconds before reading anything.
3. **Read the page.** Look for meaningful content — headings, data, elements.
   If the page looks empty or shows a spinner:
   - Wait longer, then read again.
   - Still empty? Wait once more and **screenshot** to see what's actually on screen.
4. Only after three read attempts with waits between them, conclude the page hasn't
   loaded — and note it as transient, not a permanent failure.

**The default assumption is that the app works.** A blank read almost always means
you were too fast.

## Testing behavior

- **Read the live UI as truth.** Use exact on-screen labels; quote messages verbatim.
- **Observe before you judge.** Wait for toasts, redirects, and async updates before
  deciding pass/fail. When in doubt, screenshot.
- **Derive results only from what's observable.** If nothing on screen confirms the
  outcome, your confidence is Low.

If the MCP server changes, re-map the capabilities above to the new tools and continue.
