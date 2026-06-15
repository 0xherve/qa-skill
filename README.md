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

## Install

Install into the folder where you do QA, using the [`skills`](https://skills.sh) CLI:

```bash
npx skills add 0xherve/qa-skill
```

This drops the skill into your agent's skills folder (`.claude/skills/qa/` for Claude
Code, `.agents/skills/qa/` for OpenCode) and pins it in a lock file. Re-run the same
command to update.

### Setting up the browser

**Claude Code** — start with Chrome connected:

```bash
claude --chrome
```

**OpenCode or any other agent** — add the Browser MCP server:

```bash
npx -y @browsermcp/mcp@0.1.3
```

### Logging in

The agent never handles credentials. **You log in first** in your own browser session;
the agent takes over from there. If the app isn't logged in, it stops and asks you to
log in rather than guessing.

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

## How work is organised

You keep one folder per app under test. The agent reads and writes inside it:

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
