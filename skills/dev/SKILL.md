---
name: dev
description: >
  Personal development workflow orchestrator. Use when starting any development
  task — bug fixes, features, refactors, or explorations. Runs a 5-stage process:
  Research, Plan, Scaffold, Build, Validate. Invoke with /motif:dev <task> for native
  task tracking, or /motif:dev werk <task> to use the werk CLI.
disable-model-invocation: true
argument-hint: "[werk] <task description>"
allowed-tools: "Read, Grep, Glob, Bash, Write, Edit, Agent, TaskCreate, TaskUpdate, TaskList, TaskGet, AskUserQuestion"
---

# Motif Development Workflow

You are running the motif workflow — a 5-stage development process with user approval gates between each stage.

## Argument Parsing

Parse $ARGUMENTS to determine the mode:

- If the first word of $ARGUMENTS is "werk", use **werk mode**: strip "werk" from the front and use the remainder as the task description. In the Scaffold stage, create tasks using `werk` CLI commands via Bash.
- Otherwise, use **native mode**: use all of $ARGUMENTS as the task description. In the Scaffold stage, create tasks using the TaskCreate tool.

Store the parsed task description and mode for use throughout the workflow.

## Complexity Assessment

Before starting, assess task complexity on a 3-point scale based on the task description:

- **Light** (quick bug fix, small tweak, config change): abbreviated research, minimal planning, 1-3 tasks
- **Medium** (feature addition, moderate refactor, new test coverage): standard research, full planning, 3-8 tasks
- **Heavy** (large refactor, new system, architecture change): deep research, thorough planning with tradeoff analysis, 8+ tasks

State your complexity assessment to the user before beginning Stage 1.

---

## Stage Execution Protocol

Run each stage sequentially. After completing each stage, present a summary of what was produced and ask the user how to proceed:

> **[Stage Name] complete.** [Brief summary of output]
>
> Ready to proceed to [Next Stage Name]?
> - **go** — proceed to next stage
> - **skip** — skip next stage
> - **redo** — re-run this stage (provide feedback)
> - **stop** — end the workflow here

Wait for the user's response before proceeding. Do not auto-advance.

---

## Stage 1: Research

Spawn the `researcher` subagent using the Agent tool. Pass it:
- The task description
- The complexity level (light, medium, or heavy)
- Any specific files, modules, or areas the user mentioned

The researcher will return a structured artifact with relevant files, patterns, git context, constraints, and risks.

Present a summary of the research findings to the user at the pause point.

---

## Stage 2: Plan

Using the research findings, think through the implementation approach. Include:

- **Approach**: What will be built/changed and how
- **Key files**: Which files will be created, modified, or deleted
- **Testing strategy**: What tests to write and how to verify correctness

Scale depth by complexity:
- **Light**: A few sentences covering approach and key files
- **Medium**: Full plan with approach, files, testing strategy, and brief tradeoff notes
- **Heavy**: Thorough plan with alternatives considered, tradeoff analysis, risk assessment, and dependency mapping

Present the plan to the user at the pause point.

---

## Stage 3: Scaffold

Create a structured task list from the plan. Each task should be a single, independently verifiable unit of work.

**Native mode** — use TaskCreate:
- Create tasks with clear subjects, descriptions, and activeForm values
- Set up dependencies between tasks using TaskUpdate with addBlockedBy/addBlocks where order matters
- Include a task for writing/updating tests
- Include a final task for validating the overall goal

**Werk mode** — use Bash to run `werk` CLI commands:
- Use the equivalent werk commands for task creation and dependency management
- Follow the same task structure as native mode

Present the task list to the user at the pause point. They may want to add, remove, or reorder tasks.

---

## Stage 4: Build

Execute the plan by working through the scaffolded tasks in order.

For each task:
1. Mark it as in_progress (TaskUpdate or werk equivalent)
2. Implement the change
3. If the task includes tests, write and run them
4. Mark it as completed
5. Brief status update to the user

If you encounter a blocker or something that deviates from the plan, stop and inform the user rather than improvising.

After all tasks are complete, present a summary of what was built.

---

## Stage 5: Validate

Verify the work against the original task description:

1. **Goal check**: Does the implementation satisfy the original request?
2. **Test verification**: Run the full test suite (or relevant subset). Report results.
3. **Diff review**: Summarize all files changed with a brief description of each change.
4. **Loose ends**: Flag anything incomplete, any TODO comments added, any known limitations.

Present the validation report. If issues are found, inform the user they can:
- Say **fix** with details to go back to Build and address specific issues
- Say **done** to accept the current state
