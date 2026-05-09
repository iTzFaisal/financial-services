# OpenCode Migration Plan

> Migrate `financial-services` from Claude Code plugin architecture to OpenCode native format.
> Target: one `.opencode/` directory + `opencode.json` + `AGENTS.md` at project root.
> No source files are modified — OpenCode files are generated alongside existing Claude Code files.

---

## 1. Target Directory Structure

```
.opencode/
├── agents/
│   ├── pitch-agent.md                   # primary (orchestrator)
│   ├── pitch-agent-researcher.md        # subagent
│   ├── pitch-agent-modeler.md           # subagent
│   ├── pitch-agent-deck-writer.md       # subagent
│   ├── market-researcher.md             # primary
│   ├── market-researcher-sector-reader.md   # subagent
│   ├── market-researcher-comps-spreader.md  # subagent
│   ├── market-researcher-note-writer.md     # subagent
│   ├── earnings-reviewer.md             # primary
│   ├── earnings-reviewer-transcript-reader.md  # subagent
│   ├── earnings-reviewer-model-updater.md      # subagent
│   ├── earnings-reviewer-note-writer.md        # subagent
│   ├── meeting-prep-agent.md            # primary
│   ├── meeting-prep-agent-profiler.md   # subagent
│   ├── meeting-prep-agent-news-reader.md    # subagent
│   ├── meeting-prep-agent-pack-writer.md    # subagent
│   ├── model-builder.md                 # primary
│   ├── model-builder-data-puller.md     # subagent
│   ├── model-builder-builder.md         # subagent
│   ├── model-builder-auditor.md         # subagent
│   ├── gl-reconciler.md                 # primary
│   ├── gl-reconciler-reader.md          # subagent
│   ├── gl-reconciler-critic.md          # subagent
│   ├── gl-reconciler-resolver.md        # subagent
│   ├── kyc-screener.md                  # primary
│   ├── kyc-screener-doc-reader.md       # subagent
│   ├── kyc-screener-rules-engine.md     # subagent
│   ├── kyc-screener-escalator.md        # subagent
│   ├── valuation-reviewer.md            # primary
│   ├── valuation-reviewer-package-reader.md  # subagent
│   ├── valuation-reviewer-valuation-runner.md # subagent
│   ├── valuation-reviewer-publisher.md      # subagent
│   ├── month-end-closer.md              # primary
│   ├── month-end-closer-ledger-reader.md    # subagent
│   ├── month-end-closer-rollforward.md      # subagent
│   ├── month-end-closer-poster.md           # subagent
│   ├── statement-auditor.md             # primary
│   ├── statement-auditor-statement-reader.md  # subagent
│   ├── statement-auditor-reconciler.md       # subagent
│   └── statement-auditor-flagger.md          # subagent
├── commands/
│   ├── 3-statement-model.md
│   ├── ai-readiness.md
│   ├── analyze-bond-basis.md
│   ├── analyze-bond-rv.md
│   ├── analyze-fx-carry.md
│   ├── analyze-option-vol.md
│   ├── analyze-swap-curve.md
│   ├── client-report.md
│   ├── client-review.md
│   ├── combs.md
│   ├── competitive-analysis.md
│   ├── dcf.md
│   ├── dd-checklist.md
│   ├── dd-prep.md
│   ├── debug-model.md
│   ├── financial-plan.md
│   ├── ic-memo.md
│   ├── lbo.md
│   ├── macro-rates.md
│   ├── portfolio.md
│   ├── ppt-template.md
│   ├── proposal.md
│   ├── rebalance.md
│   ├── research-equity.md
│   ├── returns.md
│   ├── review-fi-portfolio.md
│   ├── screen-deal.md
│   ├── source.md
│   ├── tlh.md
│   ├── unit-economics.md
│   └── value-creation.md
└── skills/
    ├── 3-statement-model/SKILL.md
    ├── accrual-schedule/SKILL.md
    ├── ai-readiness/SKILL.md
    ├── audit-xls/SKILL.md
    ├── bond-futures-basis/SKILL.md
    ├── bond-relative-value/SKILL.md
    ├── break-trace/SKILL.md
    ├── buyer-list/SKILL.md
    ├── catalyst-calendar/SKILL.md
    ├── clean-data-xls/SKILL.md
    ├── client-report/SKILL.md
    ├── client-review/SKILL.md
    ├── competitive-analysis/SKILL.md
    ├── comps-analysis/SKILL.md
    ├── datapack-builder/SKILL.md
    ├── dcf-model/SKILL.md
    ├── dd-checklist/SKILL.md
    ├── dd-meeting-prep/SKILL.md
    ├── deal-screening/SKILL.md
    ├── deal-sourcing/SKILL.md
    ├── deck-refresh/SKILL.md
    ├── earnings-analysis/SKILL.md
    ├── earnings-preview/SKILL.md
    ├── earnings-preview-beta/SKILL.md
    ├── equity-research/SKILL.md
    ├── financial-plan/SKILL.md
    ├── fixed-income-portfolio/SKILL.md
    ├── funding-digest/SKILL.md
    ├── fx-carry-trade/SKILL.md
    ├── gl-recon/SKILL.md
    ├── ib-check-deck/SKILL.md
    ├── ic-memo/SKILL.md
    ├── idea-generation/SKILL.md
    ├── initiating-coverage/SKILL.md
    ├── investment-proposal/SKILL.md
    ├── kyc-doc-parse/SKILL.md
    ├── kyc-rules/SKILL.md
    ├── lbo-model/SKILL.md
    ├── macro-rates-monitor/SKILL.md
    ├── model-update/SKILL.md
    ├── morning-note/SKILL.md
    ├── nav-tieout/SKILL.md
    ├── option-vol-analysis/SKILL.md
    ├── pitch-deck/SKILL.md
    ├── portfolio-monitoring/SKILL.md
    ├── portfolio-rebalance/SKILL.md
    ├── ppt-template-creator/SKILL.md
    ├── pptx-author/SKILL.md
    ├── returns-analysis/SKILL.md
    ├── roll-forward/SKILL.md
    ├── sector-overview/SKILL.md
    ├── skill-creator/SKILL.md
    ├── strip-profile/SKILL.md
    ├── swap-curve-strategy/SKILL.md
    ├── tax-loss-harvesting/SKILL.md
    ├── teaser/SKILL.md
    ├── thesis-tracker/SKILL.md
    ├── unit-economics/SKILL.md
    ├── value-creation-plan/SKILL.md
    ├── variance-commentary/SKILL.md
    └── xlsx-author/SKILL.md
```

---

## 2. Agent Conversion — Full Mapping

### 2.1 Claude tools → OpenCode permission mapping

```
Read        → read:     allow
Write       → edit:     allow
Edit        → edit:     allow
Grep        → grep:     allow
Glob        → glob:     allow
Bash        → bash:     allow
Skill       → skill:    allow
WebFetch    → webfetch: allow
WebSearch   → websearch: allow
```

**MCP tools (`mcp__<server>__*`)** are NOT mapped to permissions. They become available automatically when the MCP server is configured in `opencode.json`. The agent system prompt still references them by name — OpenCode handles tool availability via server connectivity, not explicit permission grants.

**OpenCode permission keys** not present in Claude but supported: `list`, `task`, `external_directory`, `todowrite`, `lsp`, `question`, `doom_loop`. These are not set (inherit default) unless the Claude agent had an equivalent.

### 2.2 Primary Agents (Orchestrators)

For each entry in `plugins/agent-plugins/`, create `agents/<slug>.md` with:

| Action | Detail |
|---|---|
| Source | `plugins/agent-plugins/<slug>/agents/<slug>.md` |
| Filename | `<slug>.md` |
| `mode` | `primary` |
| `name` | removed (filename = name) |
| `tools` | mapped to `permission` block |
| `model` | not set (use default from opencode.json) |
| Body | kept verbatim |

#### Per-Agent Tool → Permission Mapping

| Agent slug | Claude tools | OpenCode permission |
|---|---|---|
| pitch-agent | Read, Write, Edit, Grep, Glob, Bash, mcp__capiq__*, mcp__factset__*, mcp__pitchbook__* | `read: allow, edit: allow, grep: allow, glob: allow, bash: allow` |
| market-researcher | Read, Write, Edit, mcp__capiq__*, mcp__factset__* | `read: allow, edit: allow` |
| earnings-reviewer | Read, Write, Edit, Grep, mcp__aiera__*, mcp__capiq__*, mcp__factset__*, mcp__mtnewswire__* | `read: allow, edit: allow, grep: allow` |
| meeting-prep-agent | Read, Write, Edit, Grep, mcp__capiq__*, mcp__crm__* | `read: allow, edit: allow, grep: allow` |
| model-builder | Read, Write, Edit, Grep, Glob, Bash, mcp__capiq__*, mcp__factset__*, mcp__daloopa__*, mcp__morningstar__* | `read: allow, edit: allow, grep: allow, glob: allow, bash: allow` |
| gl-reconciler | Read, Grep, Glob, mcp__internal-gl__*, mcp__subledger__* | `read: allow, grep: allow, glob: allow` |
| kyc-screener | Read, Grep, Glob, mcp__screening__* | `read: allow, grep: allow, glob: allow` |
| valuation-reviewer | Read, Grep, Glob, mcp__capiq__*, mcp__pitchbook__*, mcp__chronograph__* | `read: allow, grep: allow, glob: allow` |
| month-end-closer | Read, Write, Edit, Grep, Glob, mcp__internal-gl__* | `read: allow, edit: allow, grep: allow, glob: allow` |
| statement-auditor | Read, Grep, Glob, mcp__internal-gl__*, mcp__subledger__* | `read: allow, grep: allow, glob: allow` |

**Note:** Each agent's actual tools vary. The migration script reads the `tools:` line from the source frontmatter and maps each tool dynamically. The table above reflects the common patterns. Some agents may differ — the script handles this generically.

### 2.3 CMA Subagents → OpenCode Subagents

For each `managed-agent-cookbooks/<slug>/subagents/<role>.yaml`, create `agents/<slug>-<role>.md`.

#### Subagent Name Convention

| Host agent | YAML `name:` field | OpenCode filename |
|---|---|---|
| gl-reconciler | gl-reconciler-reader | gl-reconciler-reader.md |
| gl-reconciler | gl-reconciler-critic | gl-reconciler-critic.md |
| gl-reconciler | gl-reconciler-resolver | gl-reconciler-resolver.md |
| pitch-agent | pitch-agent-researcher | pitch-agent-researcher.md |
| pitch-agent | pitch-agent-modeler | pitch-agent-modeler.md |
| pitch-agent | pitch-agent-deck-writer | pitch-agent-deck-writer.md |

(Filenames derived from the `name:` field in each subagent YAML.)

#### Subagent YAML → OpenCode Frontmatter Mapping

| YAML field | OpenCode frontmatter | Notes |
|---|---|---|
| `name:` | → filename | e.g., `gl-reconciler-reader` → `gl-reconciler-reader.md` |
| `model:` | `model:` | map to provider-prefixed form (see §2.4) |
| `system.text:` | body (after `---`) | pipe text into markdown body |
| `tools[].configs[].name:` | `permission:` | map each enabled tool |
| `mcp_servers[]` | not in frontmatter | configured in `opencode.json` |
| `skills[]` | not in frontmatter | all skills live in `.opencode/skills/` |
| `output_schema` | dropped | CMA-specific; no OpenCode equivalent |
| (none) | `mode: subagent` | always set for subagents |
| (none) | `description:` | auto-generated from role: `"<role> worker for <agent>"` |

#### Subagent Permission Derivation

Read `tools[].configs[]` entries where `enabled: true`:

| config name | OpenCode permission |
|---|---|
| read | `read: allow` |
| write | `edit: allow` |
| edit | `edit: allow` |
| grep | `grep: allow` |
| glob | `glob: allow` |

If `default_config.enabled: true`, all known tools are `allow`. If `default_config.enabled: false`, only explicitly `enabled: true` configs are mapped; unmapped ones default to system default (typically `ask` or `deny`).

**Security maintains:** Reader subagents (no write/edit) → no `edit` permission. Resolver/writer subagents → `edit: allow` (only one per agent, as in original).

### 2.4 Model Name Mapping

Claude Code CMA model IDs need provider prefix for OpenCode:

| Claude CMA `model:` | OpenCode `model:` |
|---|---|
| `claude-opus-4-7` | `anthropic/claude-opus-4-20250514` |
| `claude-sonnet-4-7` | `anthropic/claude-sonnet-4-20250514` |
| `claude-haiku-4-5` | `anthropic/claude-haiku-4-5-20250514` |

If the model is not in this list, pass through as-is with `anthropic/` prefix.

Primary agents (from agent-plugins) do NOT set `model:` — they inherit the project default from `opencode.json`.

---

## 3. Skill Migration

### Source → Target

Skills are collected from ALL sources and deduplicated by directory name:

| Source pattern | How collected |
|---|---|
| `plugins/vertical-plugins/*/skills/<name>/` | primary source of truth |
| `plugins/partner-built/*/skills/<name>/` | partner skills |
| `plugins/agent-plugins/*/skills/<name>/` | SKIP — these are bundled copies |

### Deduplication rule

If the same skill name exists in multiple verticals, the first one found wins (vertical-plugins take priority over partner-built). Print a warning.

### Copy operation

For each unique skill directory:
```
cp -r <source-path>/ .opencode/skills/<skill-name>/
```

The SKILL.md format is **identical** — no transformation needed. OpenCode reads the same `name:`, `description:` frontmatter plus markdown body.

### Skill inventory (expected count: ~56-60)

These include all skills from:
- `financial-analysis` (13 skills)
- `investment-banking` (5 skills)
- `equity-research` (9 skills)
- `private-equity` (10 skills)
- `wealth-management` (6 skills)
- `fund-admin` (6 skills)
- `operations` (2 skills)
- `lseg` (partner, 8 skills)
- `spglobal` (partner, 3 skills)

The script discovers them dynamically by walking directories — no hardcoded list.

### Skill support files

If a skill directory contains subdirectories (`references/`, `scripts/`, `assets/`, `templates/`), they are copied as-is. OpenCode skills support the same optional subdirectories as Claude Code skills.

---

## 4. Command Migration

### Source → Target

Commands are collected from:

| Source | Pattern |
|---|---|
| `plugins/vertical-plugins/*/commands/*.md` | all vertical commands |
| `plugins/partner-built/*/commands/*.md` | partner commands |

Agent-plugins do NOT have commands (they're in verticals only).

### Frontmatter Transformation

| Claude field | OpenCode field | Action |
|---|---|---|
| `description:` | `description:` | copy as-is |
| `argument-hint:` | removed | OpenCode uses `$ARGUMENTS` variable in body |
| (none) | `agent:` | set to `build` (OpenCode's full-access default agent) |
| (none) | `model:` | not set (inherit default) |

### Body adaptation

In the markdown body, replace patterns like:
- `"[company name or ticker]"` → `$ARGUMENTS`
- `"Run the full test suite..."` → keep as-is (this is the prompt template)

OpenCode supports `$ARGUMENTS`, `$1`, `$2` etc. for argument passing.

### Duplicate command names

If two verticals have commands with the same base filename, the first found wins. Log a warning with the conflicting paths. In practice, the current codebase has no duplicates.

---

## 5. opencode.json — Consolidated Config

### Structure

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  
  // MCP servers — consolidated from all .mcp.json files
  "mcp": {
    "daloopa": {
      "type": "http",
      "url": "https://mcp.daloopa.com/server/mcp"
    },
    "morningstar": {
      "type": "http",
      "url": "https://mcp.morningstar.com/mcp"
    },
    "sp-global": {
      "type": "http",
      "url": "https://kfinance.kensho.com/integrations/mcp"
    },
    "factset": {
      "type": "http",
      "url": "https://mcp.factset.com/mcp"
    },
    "moodys": {
      "type": "http",
      "url": "https://api.moodys.com/genai-ready-data/m1/mcp"
    },
    "mtnewswire": {
      "type": "http",
      "url": "https://vast-mcp.blueskyapi.com/mtnewswires"
    },
    "aiera": {
      "type": "http",
      "url": "https://mcp-pub.aiera.com"
    },
    "lseg": {
      "type": "http",
      "url": "https://api.analytics.lseg.com/lfa/mcp"
    },
    "pitchbook": {
      "type": "http",
      "url": "https://premium.mcp.pitchbook.com/mcp"
    },
    "chronograph": {
      "type": "http",
      "url": "https://ai.chronograph.pe/mcp"
    },
    "egnyte": {
      "type": "http",
      "url": "https://mcp-server.egnyte.com/mcp"
    }
  },

  // Global permission defaults (all agents inherit unless overridden)
  "permission": {
    "edit": "ask",
    "bash": "ask",
    "external_directory": "ask"
  },

  // Agent registrations (primary agents)
  "agent": {
    "pitch-agent": {
      "description": "Comps, precedents, LBO to a branded pitch deck, end to end",
      "mode": "primary"
    },
    "market-researcher": {
      "description": "Sector or theme to industry overview, competitive landscape, peer comps",
      "mode": "primary"
    }
    // ... (all 11 agents — descriptions from marketplace.json)
  }
}
```

### MCP server consolidation

The script reads all `.mcp.json` files:
1. `plugins/vertical-plugins/financial-analysis/.mcp.json` (canonical — 11 servers)
2. `plugins/partner-built/lseg/.mcp.json`
3. `plugins/partner-built/spglobal/.mcp.json`
4. Any `plugins/vertical-plugins/*/.mcp.json` that are non-empty

Servers are deduplicated by name. The `financial-analysis` canonical MCP takes priority.

Env-var URLs from CMA cookbooks (`${GL_MCP_URL}`, `${SUBLEDGER_MCP_URL}`) are NOT included in `opencode.json` — they are deployment-specific and should be configured in the user's global `~/.config/opencode/opencode.json` or set via environment.

---

## 6. AGENTS.md — Project Rules File

Generated from `CLAUDE.md` with updated paths:

```markdown
# Financial Services — OpenCode Configuration

## Repository Structure

```
├── .opencode/
│   ├── agents/               #   named agents (primary + subagent workers)
│   ├── commands/             #   slash commands
│   └── skills/               #   reusable skill instructions
├── opencode.json             #   MCP servers + global config
├── plugins/
│   ├── agent-plugins/        #   original Claude Code agent sources
│   ├── vertical-plugins/     #   original Claude Code skill/command sources
│   └── partner-built/        #   partner plugins (LSEG, S&P Global)
├── managed-agent-cookbooks/  #   CMA deployment manifests
├── claude-for-msft-365-install/
└── scripts/
```

## Development Workflow

1. **Edit skills** in `plugins/vertical-plugins/*/skills/<name>/SKILL.md` (source of truth)
2. **Run migration script** to propagate changes to `.opencode/skills/`:
   ```
   python3 scripts/migrate-to-opencode.py
   ```
3. **Test agents** by switching to the agent in OpenCode TUI
4. **Test commands** with `/command-name` in OpenCode

## Agent Reference

| Agent | Mode | Description |
|---|---|---|
| pitch-agent | primary | Comps, precedents, LBO → branded pitch deck |
| market-researcher | primary | Sector primer + competitive landscape + ideas |
| earnings-reviewer | primary | Earnings call → model update → note draft |
| meeting-prep-agent | primary | Client briefing pack |
| model-builder | primary | DCF, LBO, 3-statement models live in Excel |
| gl-reconciler | primary | GL ↔ subledger reconciliation |
| kyc-screener | primary | KYC document parsing + rules engine |
| valuation-reviewer | primary | GP package → valuation → LP reporting |
| month-end-closer | primary | Accruals, roll-forwards, variance commentary |
| statement-auditor | primary | Pre-distribution LP statement audit |

Each primary agent has depth-1 subagent workers (matching the CMA cookbook pattern).
```

---

## 7. Migration Script Design

### Script: `scripts/migrate-to-opencode.py`

#### CLI
```bash
python3 scripts/migrate-to-opencode.py [--dry-run] [--force]
```
- `--dry-run`: print what would be done without writing files
- `--force`: overwrite existing `.opencode/` without prompt

#### Phases (sequential, early-exit on error)

```
Phase 1: Collect sources
  ├── Walk plugins/vertical-plugins/*/skills/*/ for skills
  ├── Walk plugins/partner-built/*/skills/*/ for skills
  ├── Walk plugins/agent-plugins/*/agents/*.md for agents
  ├── Walk managed-agent-cookbooks/*/subagents/*.yaml for subagents
  ├── Walk plugins/vertical-plugins/*/commands/*.md for commands
  ├── Walk plugins/partner-built/*/commands/*.md for commands
  └── Walk plugins/*/.mcp.json for MCP servers

Phase 2: Validate
  ├── Every agent.md has YAML frontmatter with description
  ├── Every subagent.yaml has name + system.text
  ├── Every skill/SKILL.md has name + description
  └── No duplicate command names (warn if found)

Phase 3: Transform
  ├── Map agent tools → permissions
  ├── Map subagent YAML → markdown frontmatter
  ├── Map command argument-hint → $ARGUMENTS
  ├── Map MCP servers → opencode.json format
  └── Map CLAUDE.md → AGENTS.md

Phase 4: Write
  ├── Create .opencode/agents/ directory
  ├── Create .opencode/commands/ directory
  ├── Create .opencode/skills/ directory
  ├── Write each transformed agent file
  ├── Write each transformed command file
  ├── Copy each skill directory tree
  ├── Write opencode.json
  └── Write AGENTS.md
```

#### Core Functions (pseudocode)

```python
def collect_skills() -> dict[str, Path]:
    """Walk vertical-plugins + partner-built, return {name: source_path} deduplicated."""

def convert_agent(source_md: Path, dest_dir: Path) -> Path:
    """Read agent.md, transform frontmatter, write to .opencode/agents/<slug>.md"""

def convert_subagent(source_yaml: Path, dest_dir: Path) -> Path:
    """Read subagent yaml, generate markdown with frontmatter, write to .opencode/agents/<name>.md"""

def convert_command(source_md: Path, dest_dir: Path) -> Path:
    """Read command.md, transform frontmatter, write to .opencode/commands/<name>.md"""

def build_opencode_json(mcp_servers: dict, agents: list) -> str:
    """Generate opencode.json with MCP servers, global permissions, agent registry."""

def build_agents_md() -> str:
    """Generate AGENTS.md from CLAUDE.md with updated structure."""
```

#### Error Handling

| Scenario | Behavior |
|---|---|
| `.opencode/` already exists without `--force` | prompt user: `Overwrite? [y/N]` |
| Agent frontmatter missing `description` | warn, generate from slug |
| Subagent YAML missing `system.text` | error, skip |
| Skill missing `SKILL.md` | warn, skip directory |
| Duplicate command name | warn, use first, log both paths |
| MCP server URL conflict | warn, use first found |
| Cannot parse YAML/JSON | error with file path, skip item |

---

## 8. What Stays (No Changes)

These files and directories are NOT touched by the migration:

- `plugins/` — all agent-plugins, vertical-plugins, partner-built
- `managed-agent-cookbooks/` — CMA manifests preserved
- `scripts/check.py`, `sync-agent-skills.py` — still work for the Claude Code side
- `scripts/validate.py`, `orchestrate.py`, `deploy-managed-agent.sh`, `test-cookbooks.sh`
- `.claude-plugin/marketplace.json`
- `claude-for-msft-365-install/`
- `.gitignore`
- `LICENSE`, `README.md`

The migration is **additive** — OpenCode files are created alongside the existing Claude Code files. Both can coexist.

---

## 9. Verification Checklist

After running the migration script, verify:

- [ ] `ls .opencode/agents/` shows 11 primary + ~30 subagent `.md` files
- [ ] `ls .opencode/commands/` shows ~30 command `.md` files
- [ ] `ls .opencode/skills/` shows ~55 skill directories with `SKILL.md` inside each
- [ ] `opencode.json` is valid JSON (run `python3 -c "import json; json.load(open('opencode.json'))"`)
- [ ] `AGENTS.md` exists at project root
- [ ] Each agent `.md` has `mode:` (primary or subagent) in frontmatter
- [ ] Each agent `.md` has `permission:` block (mapped from tools)
- [ ] Each command `.md` has `description:` and `agent:` in frontmatter
- [ ] Each skill `SKILL.md` has `name:` and `description:` in frontmatter
- [ ] No agent `.md` retains the old `tools:` field
- [ ] No command `.md` retains the old `argument-hint:` field
- [ ] Open `opencode.json` — MCP section has all 11+ servers from `financial-analysis/.mcp.json`
- [ ] Run `python3 scripts/check.py` — should still pass (Claude Code side unchanged)
- [ ] OpenCode can discover agents: run `opencode agent list`
- [ ] OpenCode can discover skills: a skill-requiring prompt triggers skill loading
- [ ] OpenCode can run a command: `/dcf` triggers the DCF workflow

---

## 10. Rollback

To revert: delete the generated files.

```bash
rm -rf .opencode/
rm -f opencode.json
rm -f AGENTS.md
```

The original Claude Code project is untouched.

---

## 11. Future Considerations

- **CMA cookbooks** could be regenerated from OpenCode agents if needed (reverse migration script).
- **The sync-agent-skills.py** pattern could be reversed: edit skills in `.opencode/skills/` and sync back to `vertical-plugins/`.
- **The `check.py`** script could gain an OpenCode mode that validates `.opencode/` structure.
- **Partner plugins** (LSEG, S&P Global) could ship `.opencode/` configs alongside their `.claude-plugin/` manifests for dual support.
- **MCP env-var interpolation** (`${GL_MCP_URL}`) used in CMA cookbooks is deployment-specific; users configure these in their global OpenCode config or `.env`.
