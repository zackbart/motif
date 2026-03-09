# Motif — Claude Code Plugin

A cross-platform development workflow plugin. Version 0.3.2.

## Project structure

- `skills/` — universal skills (work via npx skills and as slash commands)
  - `dev/` — 5-stage workflow orchestrator (Research, Plan, Scaffold, Build, Validate)
  - `optimize-prompt/` — prompt engineering assistant for Claude models
  - `second-opinion/` — spawns adversarial critic subagent to pressure-test plans
  - `update-docs/` — documentation auditor with scanner/writer subagents
- `agents/` — Claude Code subagents (Sonnet, read-only unless noted)
  - `researcher.md` — codebase exploration for the dev workflow
  - `critic.md` — adversarial plan critique for second-opinion
  - `docs-scanner.md` — read-only documentation auditor
  - `docs-writer.md` — applies approved doc updates (has write access)
- `hooks/hooks.json` — SessionStart hook for workflow reminders
- `AGENTS.md` — Codex CLI session priming (mirrors CLAUDE.md intent)
- `.claude-plugin/` — plugin.json and marketplace.json manifests

## Conventions

- Skills use YAML frontmatter with: `name`, `description`, `allowed-tools`, `argument-hint`, `disable-model-invocation`
- Subagents use YAML frontmatter with: `name`, `description`, `tools`, `model`, `maxTurns`
- All subagents default to `model: sonnet` to keep costs down
- Read-only subagents get `tools: Read, Grep, Glob, Bash` — no Write or Edit
- Skills that delegate to subagents should pass a full briefing, not duplicate the subagent's instructions
- Version is tracked in three places: `plugin.json`, `marketplace.json`, `skills/dev/SKILL.md` — keep them in sync

## When editing this project

- Bump version in all three locations when making significant changes
- Keep AGENTS.md in sync with CLAUDE.md for Codex CLI compatibility
- Skills should be cross-platform — don't assume Claude Code-specific features in skill instructions
- Subagents are Claude Code-only — keep them in `agents/`, not inside skills
- Test skill discoverability with `npx skills add --list /path/to/motif`
- README.md project structure section should reflect actual directory layout
