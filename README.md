# 6-Agent Parallel Code Review

[日本語版はこちら / Japanese](./README_JP.md)

A 6-agent parallel code review system for Claude Code.

## Overview

Execute code reviews from 6 specialized perspectives simultaneously with a single command.

## 6 Review Perspectives

| Agent | Role | Output |
|-------|------|--------|
| **code-reviewer** | CLAUDE.md compliance, bug detection | 0-100 score |
| **comment-analyzer** | Comment accuracy, documentation quality | Evaluation report |
| **pr-test-analyzer** | Test coverage | 1-10 score |
| **silent-failure-hunter** | Error handling gaps | CRITICAL/HIGH/MEDIUM |
| **type-design-analyzer** | Type design, invariants | 4-dimension evaluation |
| **code-simplifier** | Complexity reduction, simplification | Refactoring suggestions |

## Installation

```bash
# Copy the commands folder to your Claude Code .claude directory
cp -r commands/* ~/.claude/commands/
```

## Usage

```bash
# Basic usage (auto-detects changed files from git diff)
/pr-review

# Specify files
/pr-review src/app.js src/utils.js

# Specify directory
/pr-review src/
```

## Output Format

```markdown
# PR Review Summary

## Critical Issues (X found)
- [agent-name]: Issue description [file:line]

## Important Issues (X found)
- [agent-name]: Issue description [file:line]

## Suggestions (X found)
- [agent-name]: Suggestion [file:line]

## Strengths
- Positive observations

## Score Summary
| Aspect | Score |
|--------|-------|
| Code Quality | X/100 |
| Test Coverage | X/10 |
| Type Design | 4-dimension result |
```

## Requirements

- Claude Code CLI
- Git (for auto-detection of changed files)

## License

MIT
