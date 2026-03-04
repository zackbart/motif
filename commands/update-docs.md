---
name: update-docs
description: >
  Update project documentation to reflect the current state of the codebase.
  Works with any project — detects and updates whatever documentation exists.
disable-model-invocation: true
---

# Documentation Updater

Update this project's documentation to accurately reflect the current state of the code.

## Process

### 1. Discover Documentation

Find all documentation in the project:
- README files (README.md, README.rst, etc.)
- docs/ or documentation/ directories
- API documentation (OpenAPI specs, JSDoc, rustdoc, pydoc)
- CHANGELOG, CONTRIBUTING, ARCHITECTURE files
- Inline documentation conventions (docstrings, comments)
- CLAUDE.md files (.claude/CLAUDE.md, CLAUDE.md at root)
- Wiki or GitHub Pages source if present

Report what you found to the user before proceeding.

### 2. Assess What Needs Updating

For each documentation file, check:
- Does it reference files, functions, or APIs that no longer exist?
- Does it miss recently added features or modules?
- Are code examples still accurate?
- Are installation or setup instructions current?
- Are dependency versions accurate?

### 3. Propose Changes

Present a summary of proposed updates organized by file. For each change:
- What is currently wrong or outdated
- What the updated content should say
- Why the change is needed

Wait for user approval before making changes.

### 4. Apply Updates

Make the approved changes. For each file:
- Preserve the existing style and tone
- Do not restructure documentation unless the user requests it
- Keep changes minimal and focused — update what is wrong, do not rewrite what is fine
- If a section is significantly outdated, note this to the user rather than silently rewriting it

### 5. Summary

Report what was updated, with a file-by-file list of changes made.
