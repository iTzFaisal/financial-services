# Financial Services — OpenCode Configuration

## Repository Structure

```
├── .opencode/
│   ├── agents/               #   named agents (primary + subagent workers)
│   ├── commands/             #   slash commands
│   └── skills/               #   reusable skill instructions
├── opencode.json             #   MCP servers + global config
└── scripts/
```

## Development Workflow

1. **Edit agents** in `.opencode/agents/<name>.md` (source of truth)
2. **Edit skills** in `.opencode/skills/<name>/SKILL.md`
3. **Edit commands** in `.opencode/commands/<name>.md`
4. **Test agents** by switching to the agent in OpenCode TUI
5. **Test commands** with `/command-name` in OpenCode

## Agent Reference

| Agent | Mode | Description |
|---|---|---|
| earnings-reviewer | primary | Earnings call and filings to model update to note draft |
| gl-reconciler | primary | Finds breaks, traces root cause, routes for sign-off |
| kyc-screener | primary | Parses onboarding docs, runs the rules engine, flags gaps |
| market-researcher | primary | Sector or theme to industry overview, competitive landscape, peer comps, and ideas shortlist |
| meeting-prep-agent | primary | Briefing pack before every client meeting |
| model-builder | primary | DCF, LBO, 3-statement, comps - live in Excel |
| month-end-closer | primary | Accruals, roll-forwards, variance commentary |
| pitch-agent | primary | Comps, precedents, LBO to a branded pitch deck, end to end |
| statement-auditor | primary | Audits pre-generated LP statements before distribution |
| valuation-reviewer | primary | Ingests GP packages, runs valuation template, stages LP reporting |

Each primary agent has depth-1 subagent workers (matching the CMA cookbook pattern).
