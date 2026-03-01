---
name: copilot-cli
description: "Invoke GitHub Copilot CLI for coding tasks — code generation, refactoring, debugging, and explanations"
metadata:
  openclaw:
    emoji: "🤖"
    requires:
      anyBins: ["copilot", "gh"]
    install:
      - id: copilot-standalone
        kind: download
        label: "GitHub Copilot CLI (standalone)"
        bins: ["copilot"]
        url: "https://copilot.ai/install"
      - id: gh-copilot
        kind: brew
        label: "GitHub Copilot (gh extension)"
        bins: ["gh"]
        formula: "gh"
---

# GitHub Copilot CLI Integration

Use this skill to delegate coding tasks to GitHub Copilot CLI — a terminal-based AI coding agent that can read, write, and execute code.

## When to Use

- **Code generation**: "Create a Python script that parses CSV files and generates charts"
- **Refactoring**: "Refactor this module to use dependency injection"
- **Debugging**: "Figure out why this test is failing and fix it"
- **Explanations**: "Explain how this authentication flow works"
- **Code review**: "Review the changes in this PR"

## How to Invoke

### Option 1: Standalone Copilot CLI (Recommended)

Use the `exec` tool to run `copilot` with a task description:

```bash
# Ask Copilot to perform a coding task
copilot -m "Create a TypeScript function that calculates moving averages for stock prices"

# Work on a specific directory
cd /path/to/project && copilot -m "Add unit tests for the parser module"

# Use with a specific model for better results
copilot -m "Refactor this to use async/await" --model claude-sonnet-4
```

### Option 2: gh copilot (GitHub CLI extension)

```bash
# Explain code
gh copilot explain "what does this regex do: ^(?:[a-z0-9]+(?:-[a-z0-9]+)*\.)+[a-z]{2,}$"

# Suggest a command
gh copilot suggest "find all TypeScript files with TODO comments"
```

## Best Practices

1. **Be specific**: Give Copilot clear, detailed task descriptions
2. **Set the working directory**: `cd` to the project root before invoking
3. **Review results**: Always check what Copilot produced before committing
4. **Chain with tests**: After code generation, run the test suite to verify
5. **Use for heavy lifting**: Delegate large refactors, boilerplate, and complex algorithms

## Example Workflow

```bash
# 1. Navigate to project
cd /path/to/project

# 2. Ask Copilot to implement a feature
copilot -m "Add a rate limiter middleware to the Express API in src/middleware/"

# 3. Verify it works
npm test

# 4. Report results back
echo "Feature implemented and tests pass"
```

## Integration with OpenClaw Agents

The **Builder** agent can use this skill to:
- Accept coding tasks from Deek (main agent)
- Invoke Copilot CLI for implementation
- Run tests to verify
- Report results back via `sessions_send`

This creates a chain: **Deek** → delegates to **Builder** → invokes **Copilot CLI** → results flow back.
