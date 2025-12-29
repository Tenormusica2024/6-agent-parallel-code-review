---
description: "6エージェント並列コードレビュー実行"
argument-hint: "[all|comments|tests|errors|types|code|simplify] [対象ファイル/ディレクトリ]"
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Task"]
---

# PR Review - 自動実行版

**引数:** "$ARGUMENTS"

## 即座に6エージェント並列レビューを実行

以下の6つの専門エージェントを**並列で起動**してレビューを実行してください:

### 起動するエージェント（全て並列でTaskツール実行）

1. **comment-analyzer** - コメント精度・ドキュメント品質
   - コメントとコードの整合性検証
   - ドキュメント完全性チェック

2. **pr-test-analyzer** - テストカバレッジ（1-10スコア）
   - 行動テストカバレッジ評価
   - 重要なギャップ特定

3. **silent-failure-hunter** - エラーハンドリング漏れ検出
   - サイレント障害検出
   - catch/tryブロックレビュー

4. **type-design-analyzer** - 型設計・不変条件（4次元評価）
   - 型カプセル化分析
   - 不変条件の適切な表現確認

5. **code-reviewer** - 一般的コード品質（0-100スコア）
   - CLAUDE.md準拠チェック
   - バグ・問題検出

6. **code-simplifier** - 複雑度削減・簡素化提案
   - 複雑なコードの簡素化
   - 可読性向上提案

## 実行手順

1. まず対象ファイルを特定:
   - 引数でファイル/ディレクトリ指定があればそれを使用
   - なければ `git diff --name-only` で変更ファイルを取得
   - gitリポジトリでなければ、引数で指定されたファイルをレビュー

2. **6つのTaskツールを並列で実行（各専門エージェント使用）**:
```
Task(subagent_type="code-reviewer", prompt="[ファイルリスト]をレビュー。CLAUDE.md準拠・バグ検出・0-100スコア")
Task(subagent_type="code-reviewer", prompt="comment-analyzer: [ファイルリスト]のコメント精度・ドキュメント完全性を評価")
Task(subagent_type="code-reviewer", prompt="pr-test-analyzer: [ファイルリスト]のテストカバレッジを1-10スコアで評価")
Task(subagent_type="code-reviewer", prompt="silent-failure-hunter: [ファイルリスト]のサイレント障害・catch漏れ・エラーログ品質を検出。重大度CRITICAL/HIGH/MEDIUMで分類")
Task(subagent_type="code-reviewer", prompt="type-design-analyzer: [ファイルリスト]の型正確性・不変条件・NULL安全性・データフローを各1-10で4次元評価")
Task(subagent_type="code-reviewer", prompt="code-simplifier: [ファイルリスト]の複雑度削減・重複コード・リファクタリング提案。具体的コード改善例を含める")
```

3. 結果を以下の形式で集約:

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

## 重要

- **必ず6つのTaskを並列実行すること**
- 結果が返ってきたら統合サマリーを作成
- Critical > Important > Suggestions の優先度で報告
