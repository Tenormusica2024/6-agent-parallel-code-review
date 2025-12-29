# 7-Agent Parallel Code Review

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

[Japanese / 日本語版](./README_JP.md)

**Run 7 specialized code reviews in parallel with a single command.**

> Transform your code review workflow: Get comprehensive feedback from 7 different perspectives simultaneously, reducing review time while increasing coverage.

## Why 7-Agent Parallel Review?

| Traditional Review | 7-Agent Parallel |
|-------------------|------------------|
| Single perspective | 7 specialized perspectives |
| Sequential feedback | Parallel execution |
| Manual multi-pass | One command, all insights |
| ~30 min per review | ~5 min total |

## Features

- **Parallel Execution**: All 7 agents run simultaneously
- **Security First**: Dedicated security vulnerability detection
- **Specialized Perspectives**: Each agent focuses on one domain
- **Quantified Output**: Numerical scores for objective tracking
- **Prioritized Issues**: Critical > Important > Suggestions
- **Git Integration**: Auto-detects changed files

## 7 Review Perspectives

| Agent | Focus Area | Output |
|-------|------------|--------|
| **security-analyzer** | SQLi, XSS, CSRF, auth leaks | CRITICAL/HIGH/MEDIUM |
| **code-reviewer** | CLAUDE.md compliance, bugs | 0-100 score |
| **silent-failure-hunter** | Error handling gaps | CRITICAL/HIGH/MEDIUM |
| **pr-test-analyzer** | Test coverage gaps | 1-10 score |
| **type-design-analyzer** | Type safety, invariants | 4-dimension score |
| **code-simplifier** | Complexity, refactoring | Concrete suggestions |
| **comment-analyzer** | Comment accuracy, docs | Quality report |

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/Tenormusica2024/7-agent-parallel-code-review.git

# Copy command to your Claude Code config
cp -r 7-agent-parallel-code-review/commands/* ~/.claude/commands/
```

### Usage

```bash
# Review changed files (auto-detect from git diff)
/pr-review

# Review specific files
/pr-review src/app.js src/utils.js

# Review a directory
/pr-review src/
```

## Output Example

```markdown
# PR Review Summary

## Critical Issues (3 found)
- [security-analyzer]: SQL injection via string concatenation [src/db.js:23]
- [security-analyzer]: Hardcoded API key exposed [src/config.js:5]
- [silent-failure-hunter]: Empty catch block hides errors [src/api.js:45]

## Important Issues (5 found)
- [type-design-analyzer]: Nullable type not handled [src/user.ts:78]
- [pr-test-analyzer]: No tests for edge cases [src/parser.js]
...

## Suggestions (8 found)
- [code-simplifier]: Extract repeated logic to helper [src/utils.js:12-45]
- [comment-analyzer]: Outdated comment doesn't match code [src/auth.js:30]
...

## Strengths
- Well-structured error messages
- Consistent naming conventions

## Score Summary
| Aspect | Score |
|--------|-------|
| Security | 2 CRITICAL, 1 HIGH |
| Code Quality | 72/100 |
| Test Coverage | 6/10 |
| Type Safety | 7/10 |
```

## How It Works

```
/pr-review
    │
    ├── [Parallel] security-analyzer ─────────┐
    ├── [Parallel] code-reviewer ─────────────┤
    ├── [Parallel] silent-failure-hunter ─────┤
    ├── [Parallel] pr-test-analyzer ──────────┼──> Aggregated Summary
    ├── [Parallel] type-design-analyzer ──────┤
    ├── [Parallel] code-simplifier ───────────┤
    └── [Parallel] comment-analyzer ──────────┘
```

## Requirements

- [Claude Code CLI](https://claude.ai/code)
- Git (for auto-detection of changed files)

## Configuration

The command file (`commands/pr-review.md`) can be customized:

- Modify agent prompts for your project's conventions
- Adjust output format
- Add project-specific rules

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

MIT License - see [LICENSE](LICENSE) for details.

## Related Projects

- [Claude Code](https://claude.ai/code) - AI-powered coding assistant
- [Anthropic Plugins](https://github.com/anthropics/claude-plugins-official) - Official Claude Code plugins

---

**Made with Claude Code**
