# QA Skill

A skill that turns an AI agent into a manual-QA assistant. The agent drives a browser
to **explore** a web app, **draft** test cases, and **run** them — doing the repetitive,
mechanical parts of manual testing while a human stays in the loop for judgment.

It works with [Claude Code](https://docs.claude.com/en/docs/claude-code) and any agent
that can load a skill and reach a browser through an MCP server.

## What it does

The agent does the menial QA work — navigating the app, writing test cases, executing
them, recording results — so a QA engineer can focus on deciding what's a real bug. It
never decides that for you: it gives you clear evidence, an honest read, and routes
doubt to "Uncertain" for you to settle.

Three things it can do:

| Mode | What it does |
|------|--------------|
| **explore** | Navigates the app (whole app or a module), discovers features, and drafts test cases. Drafts only — it doesn't run them. |
| **run** | Executes existing test cases, records pass/fail/uncertain/skipped, and updates their status. Flags any UI drift or uncovered features in the report. |
| **update** | Re-explores the app, fixes test cases whose steps drifted, adds cases for new features, retires removed ones. |

It finishes the whole batch and reports at the end — no babysitting each step.

## How to use it

```
/qa [explore|run|update] [scope]
```

- **mode** — `explore`, `run`, or `update`. Omit it and the agent asks.
- **scope** — `all`, or one or more module names (comma-separated). Omit it and the
  agent asks.

Examples:

```
/qa explore all                   # discover the whole app and draft test cases
/qa run documents                 # run the Documents test suite
/qa update organisation,reviews   # bring two modules' cases back in line with the app
```

If you just type `/qa`, the agent asks which mode and scope before doing anything.

## Setting up the browser MCP

The agent needs a browser it can drive, exposed through an MCP server. The skill is
tool-agnostic — it works in terms of actions (navigate, click, type, read page,
screenshot) and maps them to whatever server is configured. Any of these work:

- **Playwright MCP** (`@playwright/mcp`) — recommended; cross-browser and well-supported.
- **Chrome DevTools MCP** — drives a real Chrome instance.
- Any other browser MCP server.

### Claude Code

Add a browser MCP server, then check it's connected:

```bash
# Playwright MCP
claude mcp add playwright -- npx -y @playwright/mcp@latest

# verify
claude mcp list
```

You can also add it to `.mcp.json` in the app's repo so the whole team shares the same
setup:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

Then drop the `skills/qa/` folder where Claude Code loads skills (e.g.
`.claude/skills/qa/`) and invoke with `/qa`.

### Other agents (OpenCode, custom harnesses)

1. Register the same browser MCP server with your agent (see its MCP docs — most take a
   `command` + `args` like the JSON above).
2. Point the agent at `skills/qa/SKILL.md` as its instructions. The references under
   `skills/qa/references/` carry the detail; the agent reads them as needed.
3. Give it access to the per-app repo (below) so it can read and write test cases.

### Logging in

The agent never handles credentials. **You log in first** in the browser the MCP server
controls; the agent takes over that authenticated session. If the app isn't logged in,
it stops and asks you to log in rather than guessing.

## How work is organised

You keep one repository per app under test. The agent reads and writes inside it:

```
test-cases/            # the test suite — markdown, the source of truth
  <module>/
    README.md          # module description + summary table with current status
    TC01_<Action>.md   # one test case: steps, input, expected result, status
runs/                  # run history — JSON, easy to query and diff
  YYYY-MM-DD_HHMM/
    report.json        # run summary (totals, failures, changes, notes)
    <module>.json      # per-test-case results for that run
```

The markdown suite holds the **current** state; the `runs/` JSON holds the **history**.
Each test case is numbered per module (every module starts at TC01), so adding or
removing a module never forces a renumber.

After a run, the agent prints a results table in the chat **and** saves the run JSON.

## Statuses

| Status | Meaning |
|--------|---------|
| **Passed** | Verified working, clear evidence. |
| **Failed** | Confirmed broken — a genuine defect, not intended behaviour. |
| **Uncertain** | Ran it, but the result is ambiguous; needs a human call. |
| **Skipped** | Couldn't attempt it (upload, email, OTP) or too little signal to claim anything. |

The agent commits to Pass/Fail only when it's confident; anything shaky becomes
Uncertain, anything unobservable becomes Skipped. This keeps the "valid bug" rate high.

## Layout

```
skills/
  qa/
    SKILL.md                       # the skill the agent loads
    references/
      voice.md                     # tone — read first
      qa-workflow.md               # the three modes in detail
      test-cases-format.md         # markdown structure + writing rules
      status-confidence.md         # status enum + confidence rubric
      runs-format.md               # JSON run history + report table
      run-mechanics.md             # skipping, drift, failures, test data
      browser-actions.md           # MCP-agnostic browser capabilities
      excel-format.md              # column schemas + Excel reporting/navigation
      collaboration-guidelines.md  # behaviour — when to act, pause, hand back
```
