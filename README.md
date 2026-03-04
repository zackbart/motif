# Motif

A structured 5-stage development workflow for AI coding agents: **Research → Plan → Scaffold → Build → Validate**.

Works with [Claude Code](https://claude.com/claude-code), [OpenAI Codex CLI](https://github.com/openai/codex), and any agent platform that supports the [Agent Skills](https://developers.openai.com/codex/skills/) standard.

Motif ensures you research before coding, plan with tradeoff analysis, track tasks explicitly, and validate against the original goal.

## Installation

### Claude Code

```bash
claude plugin add zackbart/motif
```

### Codex CLI

Clone into your skills directory:

```bash
git clone https://github.com/zackbart/motif.git ~/.agents/skills/motif
```

### Other Agent Platforms

Copy the `skills/` and `commands/` directories into your agent's skill discovery path.

## Usage

Run the full workflow:

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

## Additional Commands

| Command | Description |
|---------|-------------|
| `optimize` | Prompt drafting assistant for system and agentic prompts |
| `update-docs` | Updates project documentation to reflect current codebase state |

## Platform-Specific Features

| Feature | Claude Code | Codex CLI |
|---------|-------------|-----------|
| Research subagent | Dedicated agent (model: sonnet) | Inline in main context |
| Task tracking | Native task tools | update_plan / werk CLI |
| Session hooks | SessionStart hook | AGENTS.md |
| Plugin distribution | Marketplace | Git clone |

## Project Structure

```
skills/dev/     # Core 5-stage workflow + reference guides
agents/         # Subagent definitions (Claude Code enhancement)
commands/       # Additional commands (optimize, update-docs)
hooks/          # Session lifecycle hooks (Claude Code)
AGENTS.md       # Session priming (Codex CLI)
```

## License

MIT
