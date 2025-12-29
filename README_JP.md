# 6エージェント並列コードレビュー

[English version](./README.md)

Claude Code用の6エージェント並列コードレビューシステム

## 概要

1つのコマンドで6つの専門観点から同時にコードレビューを実行します。

## 6つのレビュー観点

| エージェント | 役割 | 出力 |
|-------------|------|------|
| **code-reviewer** | CLAUDE.md準拠・バグ検出 | 0-100スコア |
| **comment-analyzer** | コメント精度・ドキュメント品質 | 評価レポート |
| **pr-test-analyzer** | テストカバレッジ | 1-10スコア |
| **silent-failure-hunter** | エラーハンドリング漏れ | CRITICAL/HIGH/MEDIUM |
| **type-design-analyzer** | 型設計・不変条件 | 4次元評価 |
| **code-simplifier** | 複雑度削減・簡素化提案 | リファクタリング案 |

## インストール

```bash
# commandsフォルダをClaude Codeの.claudeディレクトリにコピー
cp -r commands/* ~/.claude/commands/
```

## 使い方

```bash
# 基本的な使い方（git diffの変更ファイルを自動検出）
/pr-review

# 特定ファイルを指定
/pr-review src/app.js src/utils.js

# ディレクトリ指定
/pr-review src/
```

## 出力形式

```markdown
# PR Review Summary

## Critical Issues (X found)
- [エージェント名]: 問題説明 [file:line]

## Important Issues (X found)
- [エージェント名]: 問題説明 [file:line]

## Suggestions (X found)
- [エージェント名]: 提案 [file:line]

## Strengths
- 良かった点

## スコアサマリー
| 観点 | スコア |
|------|--------|
| コード品質 | X/100 |
| テストカバレッジ | X/10 |
| 型設計 | 4次元評価結果 |
```

## 動作要件

- Claude Code CLI
- Git（変更ファイル自動検出時）

## ライセンス

MIT
