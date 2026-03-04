# Motif

A structured 5-stage development workflow for AI coding agents: **Research → Plan → Scaffold → Build → Validate**.

Works with any agent that supports the [Agent Skills](https://agentskills.io) standard — including [Claude Code](https://claude.com/claude-code), [OpenAI Codex CLI](https://github.com/openai/codex), [Cursor](https://cursor.com), and [40+ other platforms](https://agentskills.io/home).

Motif ensures you research before coding, plan with tradeoff analysis, track tasks explicitly, and validate against the original goal.

## Installation

### Any Agent (via npx skills)

```bash
npx skills add zackbart/motif
```

This installs the `dev` skill to whichever agent platforms you have installed.

### Claude Code (full plugin)

```bash
claude plugin add zackbart/motif
```

Includes the core workflow plus additional commands (`optimize`, `update-docs`), a dedicated research subagent, and session hooks.

### Codex CLI

```bash
git clone https://github.com/zackbart/motif.git ~/.agents/skills/motif
```

## Usage

Invoke the `dev` skill using your agent's skill invocation method:

```
/motif:dev <task description>        # Claude Code
$dev <task description>              # Codex CLI
```

Or use [werk](https://github.com/zackbart/werk) for persistent task tracking:

```
/motif:dev werk <task description>   # Claude Code
$dev werk <task description>         # Codex CLI
```

## Workflow Stages

Each stage requires your approval before advancing to the next.

### 1. Research

Performs deep, read-only codebase exploration. Depth scales automatically based on task complexity (light / medium / heavy). On Claude Code, this can be delegated to a dedicated research subagent for efficiency.

### 2. Plan

Develops an implementation approach based on the research findings. Medium and heavy tasks include tradeoff analysis and risk assessment.

### 3. Scaffold

Creates a structured task list from the plan using the platform's available task tracking tools, or the `werk` CLI.

### 4. Build

Executes the plan by working through tasks in order. Stops and reports if blockers are encountered.

### 5. Validate

Verifies the implementation against the original request: goal check, test verification, diff review, and loose-end identification.

## Claude Code Extras

When installed as a Claude Code plugin, motif includes additional features:

| Feature | Description |
|---------|-------------|
| Research subagent | Dedicated agent on Sonnet for faster, cheaper research |
| `optimize` command | Prompt drafting assistant for system and agentic prompts |
| `update-docs` command | Updates project documentation to reflect current codebase |
| Session hooks | Reminds you about the workflow on session start |

## Platform Compatibility

| Feature | Claude Code | Codex CLI | Other Agents |
|---------|-------------|-----------|--------------|
| 5-stage workflow | Yes | Yes | Yes |
| Research subagent | Dedicated agent | Inline | Inline |
| Task tracking | Native task tools | update_plan / werk | Platform-dependent / werk |
| Extra commands | optimize, update-docs | — | — |
| Distribution | Plugin marketplace | Git clone | npx skills |

## Project Structure

```
skills/dev/          # Core 5-stage workflow (universal skill)
  references/        # Prompt engineering reference guides
agents/              # Research subagent (Claude Code)
commands/            # Additional commands (Claude Code)
hooks/               # Session lifecycle hooks (Claude Code)
AGENTS.md            # Session priming (Codex CLI)
.claude-plugin/      # Plugin manifest (Claude Code)
```

## License

MIT
