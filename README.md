# OpenCode for Financial Services

Reference agents, skills, commands, and data connectors for investment banking, equity research, private equity, wealth management, and fund administration — built for [OpenCode](https://opencode.ai).

Source: [github.com/anthropics/financial-services](https://github.com/anthropics/financial-services)
Modified for OpenCode — agents, skills, and commands adapted for the OpenCode.

> [!IMPORTANT]
> Nothing in this repository constitutes investment, legal, tax, or accounting advice. These agents draft analyst work product — models, memos, research notes, reconciliations — for review by a qualified professional. They do not make investment recommendations, execute transactions, bind risk, post to a ledger, or approve onboarding; every output is staged for human sign-off. You are responsible for verifying outputs and for compliance with the laws and regulations that apply to your firm.

## Repository Structure

```
.opencode/
├── agents/        # 10 primary agents + 30 subagent workers
├── commands/      # 47 slash commands (/dcf, /comps, /earnings, …)
└── skills/        # 66 skill modules (DCF, LBO, GL recon, KYC, …)
opencode.json      # MCP servers, global permissions, agent registry
AGENTS.md          # Development workflow + agent reference
```

Everything is file-based — markdown and JSON. No build step.

## Getting Started

### Prerequisites

- [OpenCode](https://opencode.ai) installed
- API keys for any MCP data providers you plan to use

### Setup

```bash
# Clone the repo
git clone https://github.com/anthropics/financial-services.git
cd financial-services

# OpenCode discovers .opencode/ automatically
# Agents appear in the TUI when you switch agent mode
# Skills load automatically when their trigger conditions match
# Commands are available as /command-name
```

Once inside the repo, OpenCode automatically discovers:

- **Agents** — switch to `pitch-agent`, `gl-reconciler`, `market-researcher`, etc. from the agent picker
- **Skills** — fire automatically when relevant (e.g., asking for a DCF triggers `dcf-model`)
- **Commands** — use `/dcf`, `/comps`, `/earnings`, `/ic-memo`, and 43 more slash commands

### MCP Data Providers

Configure API keys for the providers you use. OpenCode reads `opencode.json` for server URLs; set credentials in your global OpenCode config (`~/.config/opencode/opencode.json`) or via environment variables.

## Agents

Each agent is a named workflow with depth-1 subagent workers. They produce staged work product for human review — they do not publish, post, or approve.

| Function                     | Agent                | Description                                                                           |
| ---------------------------- | -------------------- | ------------------------------------------------------------------------------------- |
| **Coverage & Advisory**      | `pitch-agent`        | Comps, precedents, LBO → branded pitch deck, end to end                               |
|                              | `meeting-prep-agent` | Briefing pack before every client meeting                                             |
| **Research & Modeling**      | `market-researcher`  | Sector primer → industry overview, competitive landscape, peer comps, ideas shortlist |
|                              | `earnings-reviewer`  | Earnings call + filings → model update → note draft                                   |
|                              | `model-builder`      | DCF, LBO, 3-statement, comps — live in Excel                                          |
| **Fund Admin & Finance Ops** | `valuation-reviewer` | Ingests GP packages, runs valuation template, stages LP reporting                     |
|                              | `gl-reconciler`      | GL ↔ subledger reconciliation — finds breaks, traces root cause, routes for sign-off  |
|                              | `month-end-closer`   | Accruals, roll-forwards, variance commentary                                          |
|                              | `statement-auditor`  | Audits pre-generated LP statements before distribution                                |
| **Operations**               | `kyc-screener`       | Parses onboarding docs, runs KYC/AML rules engine, flags gaps                         |

Each primary agent is backed by 3 subagent workers following a security isolation pattern:

- **Reader** — reads untrusted documents, returns structured data only
- **Processor** — analyzes data against trusted MCP sources, read-only
- **Writer** — the only worker with edit permission, produces final output files

## Skills & Commands

Skills are organized by functional area. Every skill can be invoked explicitly or fires automatically. Commands are explicit slash triggers.

### Core Modeling & Analysis

| Skill                  | Command                 | Description                                                            |
| ---------------------- | ----------------------- | ---------------------------------------------------------------------- |
| `dcf-model`            | `/dcf`                  | DCF valuation with WACC, sensitivity tables, terminal value            |
| `comps-analysis`       | `/comps`                | Trading comps and precedent transactions with statistical benchmarking |
| `lbo-model`            | `/lbo`                  | Leveraged buyout with sources & uses, returns sensitivity              |
| `3-statement-model`    | `/3-statement-model`    | Populate and link IS/BS/CF model templates                             |
| `competitive-analysis` | `/competitive-analysis` | Competitive landscape, market positioning, peer benchmarking           |
| `audit-xls`            | `/debug-model`          | Formula audit — trace precedents, flag hardcodes, balance checks       |
| `clean-data-xls`       | —                       | Normalize messy spreadsheet data                                       |
| `deck-refresh`         | —                       | Re-link and refresh charts/tables across a presentation                |
| `ib-check-deck`        | —                       | QC investor presentations for consistency and polish                   |

### Investment Banking

| Skill              | Command           | Description                                          |
| ------------------ | ----------------- | ---------------------------------------------------- |
| `pitch-deck`       | —                 | Populate pitch deck templates from source data       |
| `strip-profile`    | —                 | One-page company profiles for pitch books            |
| `datapack-builder` | —                 | Build data packs from CIMs, filings, and market data |
| `teaser`           | `/teaser`         | Anonymous one-page company teasers                   |
| `buyer-list`       | `/buyer-list`     | Strategic and financial buyer universe               |
| `process-letter`   | `/process-letter` | Bid instructions and process correspondence          |
| `deal-tracker`     | `/deal-tracker`   | Track live deals, milestones, and action items       |

### Equity Research

| Skill                 | Command             | Description                                                    |
| --------------------- | ------------------- | -------------------------------------------------------------- |
| `earnings-analysis`   | `/earnings`         | Post-earnings quarterly update (8-12 pages with charts)        |
| `earnings-preview`    | `/earnings-preview` | Pre-earnings preview with scenario analysis                    |
| `initiating-coverage` | `/initiate`         | Full initiation report (research → model → valuation → report) |
| `model-update`        | `/model-update`     | Drop actuals into coverage models, roll estimates              |
| `morning-note`        | `/morning-note`     | Morning meeting note across coverage universe                  |
| `sector-overview`     | `/sector`           | Industry landscape and thematic research                       |
| `thesis-tracker`      | `/thesis`           | Maintain and update investment theses                          |
| `catalyst-calendar`   | `/catalysts`        | Track upcoming catalysts across coverage                       |
| `idea-generation`     | `/screen`           | Stock screening and idea sourcing                              |
| `equity-research`     | `/research-equity`  | Comprehensive equity research snapshots                        |

### Private Equity

| Skill                  | Command           | Description                                        |
| ---------------------- | ----------------- | -------------------------------------------------- |
| `deal-sourcing`        | `/source`         | Discover companies, check CRM, draft outreach      |
| `deal-screening`       | `/screen-deal`    | Pass/fail inbound CIMs and teasers                 |
| `dd-checklist`         | `/dd-checklist`   | Due diligence checklists by workstream             |
| `dd-meeting-prep`      | `/dd-prep`        | Prep for management presentations and expert calls |
| `unit-economics`       | `/unit-economics` | ARR cohorts, LTV/CAC, net retention                |
| `returns-analysis`     | `/returns`        | IRR/MOIC sensitivity tables                        |
| `ic-memo`              | `/ic-memo`        | Investment committee memo drafting                 |
| `portfolio-monitoring` | `/portfolio`      | Track portfolio company KPIs and variances         |
| `value-creation-plan`  | `/value-creation` | Post-close 100-day plans and EBITDA bridges        |
| `ai-readiness`         | `/ai-readiness`   | Portfolio AI readiness assessment                  |

### Wealth Management

| Skill                 | Command           | Description                                |
| --------------------- | ----------------- | ------------------------------------------ |
| `client-review`       | `/client-review`  | Prep for client meetings                   |
| `financial-plan`      | `/financial-plan` | Retirement, education, estate projections  |
| `portfolio-rebalance` | `/rebalance`      | Allocation drift and tax-aware rebalancing |
| `client-report`       | `/client-report`  | Client-facing performance reports          |
| `investment-proposal` | `/proposal`       | New client proposals                       |
| `tax-loss-harvesting` | `/tlh`            | TLH opportunities and wash sale management |

### Fund Administration & Operations

| Skill                 | Command | Description                                                 |
| --------------------- | ------- | ----------------------------------------------------------- |
| `gl-recon`            | —       | GL ↔ subledger reconciliation at position/transaction level |
| `break-trace`         | —       | Root-cause reconciliation breaks to source transactions     |
| `accrual-schedule`    | —       | Period-end accrual schedule with JEs                        |
| `roll-forward`        | —       | Balance-sheet roll-forward schedules                        |
| `variance-commentary` | —       | P&L and BS flux commentary over threshold                   |
| `nav-tieout`          | —       | Tie LP statements to fund NAV pack                          |
| `kyc-doc-parse`       | —       | Parse onboarding packets into structured KYC fields         |
| `kyc-rules`           | —       | KYC/AML rules grid — risk rating, rule outcomes, flags      |

### Markets & Analytics (LSEG)

| Skill                    | Command                | Description                                             |
| ------------------------ | ---------------------- | ------------------------------------------------------- |
| `bond-futures-basis`     | `/analyze-bond-basis`  | Bond futures basis, CTD, implied repo                   |
| `bond-relative-value`    | `/analyze-bond-rv`     | Bond richness/cheapness vs curves, spread decomposition |
| `fx-carry-trade`         | `/analyze-fx-carry`    | FX carry trade analysis with vol surface context        |
| `option-vol-analysis`    | `/analyze-option-vol`  | Option vol surfaces, Greeks, IV vs RV                   |
| `swap-curve-strategy`    | `/analyze-swap-curve`  | Swap curves, spreads, curve trade opportunities         |
| `fixed-income-portfolio` | `/review-fi-portfolio` | Portfolio duration, DV01, cashflows, scenario analysis  |
| `macro-rates-monitor`    | `/macro-rates`         | Macro indicators, yield curves, real rates dashboards   |

### S&P Global

| Skill                   | Command | Description                                           |
| ----------------------- | ------- | ----------------------------------------------------- |
| `tear-sheet`            | —       | Company tear sheets and one-pagers                    |
| `earnings-preview-beta` | —       | 4-5 page earnings preview with transcript analysis    |
| `funding-digest`        | —       | Weekly deal flow and capital markets activity summary |

### Utility

| Skill                  | Command         | Description                         |
| ---------------------- | --------------- | ----------------------------------- |
| `pptx-author`          | —               | Headless `.pptx` generation         |
| `xlsx-author`          | —               | Headless `.xlsx` generation         |
| `ppt-template-creator` | `/ppt-template` | Create reusable PPT template skills |
| `skill-creator`        | —               | Guide for creating new skills       |

## MCP Integrations

All data connectors are configured in `opencode.json`:

| Provider                                     | URL                                              |
| -------------------------------------------- | ------------------------------------------------ |
| [Daloopa](https://www.daloopa.com/)          | `https://mcp.daloopa.com/server/mcp`             |
| [Morningstar](https://www.morningstar.com/)  | `https://mcp.morningstar.com/mcp`                |
| [S&P Global](https://www.spglobal.com/)      | `https://kfinance.kensho.com/integrations/mcp`   |
| [FactSet](https://www.factset.com/)          | `https://mcp.factset.com/mcp`                    |
| [Moody's](https://www.moodys.com/)           | `https://api.moodys.com/genai-ready-data/m1/mcp` |
| [MT Newswires](https://www.mtnewswires.com/) | `https://vast-mcp.blueskyapi.com/mtnewswires`    |
| [Aiera](https://www.aiera.com/)              | `https://mcp-pub.aiera.com`                      |
| [LSEG](https://www.lseg.com/)                | `https://api.analytics.lseg.com/lfa/mcp`         |
| [PitchBook](https://pitchbook.com/)          | `https://premium.mcp.pitchbook.com/mcp`          |
| [Chronograph](https://www.chronograph.pe/)   | `https://ai.chronograph.pe/mcp`                  |
| [Egnyte](https://www.egnyte.com/)            | `https://mcp-server.egnyte.com/mcp`              |

MCP access may require a subscription or API key from the provider. Deployment-specific servers (internal GL, subledger, CRM, screening, NAV, portfolio) use env-var URLs — configure those in your global OpenCode config.

## Making It Yours

These are reference templates — they get better when you tune them to how your firm works.

- **Swap connectors** — edit `opencode.json` to point at your data providers and internal systems.
- **Add firm context** — drop your terminology, processes, and formatting standards into skill files.
- **Bring your templates** — `/ppt-template` teaches OpenCode your branded PowerPoint layouts.
- **Adjust agent scope** — edit `.opencode/agents/<name>.md` to match how your team runs the workflow.
- **Add your own** — follow the existing pattern: agent `.md` in `agents/`, commands in `commands/`, skills in `skills/<name>/SKILL.md`.

## Contributing

Everything is markdown and JSON. Fork, edit, PR.

- **Agents** — edit `.opencode/agents/<name>.md`
- **Skills** — edit `.opencode/skills/<name>/SKILL.md`
- **Commands** — edit `.opencode/commands/<name>.md`
- **MCP servers** — edit `opencode.json`

## License

[Apache License 2.0](./LICENSE)
