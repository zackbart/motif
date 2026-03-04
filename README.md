# Motif

A Claude Code plugin that implements a structured 5-stage development workflow: **Research → Plan → Scaffold → Build → Validate**.

Motif ensures you research before coding, plan with tradeoff analysis, track tasks explicitly, and validate against the original goal — all from within Claude Code.

## Installation

```bash
claude plugin add zackbart/motif
```

## Usage

Run the full workflow with:

```
/motif:dev <task description>
```

Or use [werk](https://github.com/zackbart/werk) for persistent task tracking:

```
/motif:dev werk <task description>
```

## Workflow Stages

Each stage requires your approval before advancing to the next.

### 1. Research

Spawns a read-only subagent to explore the codebase. Depth scales automatically based on task complexity (light / medium / heavy).

### 2. Plan

Develops an implementation approach based on the research findings. Medium and heavy tasks include tradeoff analysis and risk assessment.

### 3. Scaffold

Creates a structured task list from the plan — either using Claude Code's native task tools or the `werk` CLI.

### 4. Build

Executes the plan by working through tasks in order. Stops and reports if blockers are encountered.

### 5. Validate

Verifies the implementation against the original request: goal check, test verification, diff review, and loose-end identification.

## Additional Commands

| Command | Description |
|---------|-------------|
| `/motif:optimize` | Prompt drafting assistant for system and agentic prompts |
| `/motif:update-docs` | Updates project documentation to reflect current codebase state |

## Project Structure

```
agents/         # Subagent definitions (researcher)
commands/       # Additional commands (optimize, update-docs)
hooks/          # Session lifecycle hooks
skills/dev/     # Core 5-stage workflow + reference guides
```

## License

MIT
