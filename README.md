# Parallel Code Review for Claude Code

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

[Japanese / 日本語版](./README_JP.md)

**Run 7/12/17 specialized code reviews in parallel with a single command.**

> Transform your code review workflow: Get comprehensive feedback from multiple perspectives simultaneously, reducing review time while increasing coverage.

## Review Modes

| Mode | Command | Agents | Use Case |
|------|---------|--------|----------|
| **Standard** | `/pr-review` | 7 | Daily PR reviews, quick checks |
| **Full** | `/pr-review full` | 12 | Important features, thorough review |
| **Release** | `/pr-review release` | 17 | App release, store submission |

## Features

- **3 Review Modes**: Choose depth based on your needs
- **Parallel Execution**: All agents run simultaneously
- **Security First**: Dedicated security vulnerability detection
- **Quantified Output**: Numerical scores for objective tracking
- **Prioritized Issues**: Critical > Important > Suggestions
- **Git Integration**: Auto-detects changed files

---

## Standard Mode (7 Agents)

Daily PR reviews. Quick core quality checks.

| Agent | Focus Area | Output |
|-------|------------|--------|
| **security-analyzer** | SQLi, XSS, CSRF, auth leaks | CRITICAL/HIGH/MEDIUM |
| **code-reviewer** | CLAUDE.md compliance, bugs | 0-100 score |
| **silent-failure-hunter** | Error handling gaps | CRITICAL/HIGH/MEDIUM |
| **pr-test-analyzer** | Test coverage gaps | 1-10 score |
| **type-design-analyzer** | Type safety, invariants | 4-dimension score |
| **code-simplifier** | Complexity, refactoring | Concrete suggestions |
| **comment-analyzer** | Comment accuracy, docs | Quality report |

---

## Full Mode (12 Agents)

Comprehensive review. For important features or thorough checks.

### Additional Agents (+5)

| Agent | Focus Area | Output |
|-------|------------|--------|
| **performance-analyzer** | N+1 queries, memory leaks | HIGH/MEDIUM/LOW |
| **dead-code-hunter** | Unused code, imports | List |
| **dependency-analyzer** | Outdated/vulnerable deps | Risk report |
| **accessibility-analyzer** | ARIA, contrast, keyboard | A11y report |
| **ux-analyzer** | UX anti-patterns | Suggestions |

---

## Release Mode (17 Agents)

Full pre-release check. Includes App Store/Google Play compliance.

### Additional Agents (+5)

| Agent | Focus Area | Output |
|-------|------------|--------|
| **store-review-analyzer** | Apple/Google guidelines | Compliance report |
| **analytics-analyzer** | Tracking gaps | Coverage report |
| **localization-analyzer** | i18n, hardcoded strings | Issue list |
| **api-rate-limit-analyzer** | API limits, costs | Risk report |
| **logging-analyzer** | Log quality, leaks | Quality report |

---

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
# Standard review (7 agents)
/pr-review

# Full review (12 agents)
/pr-review full

# Release review (17 agents)
/pr-review release

# Review specific files
/pr-review src/app.js src/utils.js

# Full review on specific directory
/pr-review full src/
```

## Output Example

```markdown
# PR Review Summary (Full Mode - 12 Agents)

## Critical Issues (3 found)
- [security-analyzer]: SQL injection via string concatenation [src/db.js:23]
- [security-analyzer]: Hardcoded API key exposed [src/config.js:5]
- [silent-failure-hunter]: Empty catch block hides errors [src/api.js:45]

## Important Issues (5 found)
- [performance-analyzer]: N+1 query in user list [src/users.js:56]
- [type-design-analyzer]: Nullable type not handled [src/user.ts:78]
- [accessibility-analyzer]: Missing ARIA label [src/Button.tsx:12]
...

## Suggestions (8 found)
- [code-simplifier]: Extract repeated logic to helper [src/utils.js:12-45]
- [dead-code-hunter]: Unused import 'lodash' [src/utils.js:1]
- [ux-analyzer]: No loading indicator for async action [src/Form.tsx:45]
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
| Performance | 1 HIGH, 2 MEDIUM |
| Accessibility | 3 issues |
```

## How It Works

```
/pr-review [mode]
    │
    ├── [Parallel] security-analyzer ─────────┐
    ├── [Parallel] code-reviewer ─────────────┤
    ├── [Parallel] silent-failure-hunter ─────┤
    ├── [Parallel] pr-test-analyzer ──────────┤
    ├── [Parallel] type-design-analyzer ──────┤
    ├── [Parallel] code-simplifier ───────────┤
    ├── [Parallel] comment-analyzer ──────────┤
    │                                         │
    │   [full/release mode only]              │
    ├── [Parallel] performance-analyzer ──────┤
    ├── [Parallel] dead-code-hunter ──────────┼──> Aggregated Summary
    ├── [Parallel] dependency-analyzer ───────┤
    ├── [Parallel] accessibility-analyzer ────┤
    ├── [Parallel] ux-analyzer ───────────────┤
    │                                         │
    │   [release mode only]                   │
    ├── [Parallel] store-review-analyzer ─────┤
    ├── [Parallel] analytics-analyzer ────────┤
    ├── [Parallel] localization-analyzer ─────┤
    ├── [Parallel] api-rate-limit-analyzer ───┤
    └── [Parallel] logging-analyzer ──────────┘
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

---

**Made with Claude Code**
