# 7エージェント並列コードレビュー

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

[English](./README.md)

**1つのコマンドで7つの専門レビューを同時実行**

> コードレビューのワークフローを変革: 7つの異なる観点から同時にフィードバックを取得し、レビュー時間を短縮しながらカバレッジを向上。

## なぜ7エージェント並列レビュー？

| 従来のレビュー | 7エージェント並列 |
|---------------|------------------|
| 単一の視点 | 7つの専門視点 |
| 順次フィードバック | 並列実行 |
| 手動で複数回確認 | 1コマンドで全ての洞察 |
| レビューごとに約30分 | 合計約5分 |

## 特徴

- **並列実行**: 7つのエージェントが同時に動作
- **セキュリティ優先**: 専用のセキュリティ脆弱性検出
- **専門的な視点**: 各エージェントが1つの領域に特化
- **定量的な出力**: 客観的な追跡のための数値スコア
- **優先順位付き**: Critical > Important > Suggestions
- **Git連携**: 変更ファイルを自動検出

## 7つのレビュー観点

| エージェント | 対象領域 | 出力 |
|-------------|----------|------|
| **security-analyzer** | SQLi、XSS、CSRF、認証漏れ | CRITICAL/HIGH/MEDIUM |
| **code-reviewer** | CLAUDE.md準拠、バグ | 0-100スコア |
| **silent-failure-hunter** | エラーハンドリング | CRITICAL/HIGH/MEDIUM |
| **pr-test-analyzer** | テストカバレッジの欠落 | 1-10スコア |
| **type-design-analyzer** | 型安全性、不変条件 | 4次元スコア |
| **code-simplifier** | 複雑度、リファクタリング | 具体的な提案 |
| **comment-analyzer** | コメント精度、ドキュメント | 品質レポート |

## クイックスタート

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/Tenormusica2024/6-agent-parallel-code-review.git

# コマンドをClaude Code設定にコピー
cp -r 6-agent-parallel-code-review/commands/* ~/.claude/commands/
```

### 使い方

```bash
# 変更ファイルをレビュー（git diffから自動検出）
/pr-review

# 特定ファイルをレビュー
/pr-review src/app.js src/utils.js

# ディレクトリをレビュー
/pr-review src/
```

## 出力例

```markdown
# PR Review Summary

## Critical Issues (3 found)
- [security-analyzer]: 文字列連結によるSQLインジェクション [src/db.js:23]
- [security-analyzer]: ハードコードされたAPIキーが露出 [src/config.js:5]
- [silent-failure-hunter]: 空のcatchブロックがエラーを隠蔽 [src/api.js:45]

## Important Issues (5 found)
- [type-design-analyzer]: Nullable型が処理されていない [src/user.ts:78]
- [pr-test-analyzer]: エッジケースのテストなし [src/parser.js]
...

## Suggestions (8 found)
- [code-simplifier]: 繰り返しロジックをヘルパーに抽出 [src/utils.js:12-45]
- [comment-analyzer]: 古いコメントがコードと一致しない [src/auth.js:30]
...

## Strengths
- 適切に構造化されたエラーメッセージ
- 一貫した命名規則

## Score Summary
| 観点 | スコア |
|------|--------|
| セキュリティ | 2 CRITICAL, 1 HIGH |
| コード品質 | 72/100 |
| テストカバレッジ | 6/10 |
| 型安全性 | 7/10 |
```

## 動作の仕組み

```
/pr-review
    │
    ├── [並列] security-analyzer ─────────────┐
    ├── [並列] code-reviewer ─────────────────┤
    ├── [並列] silent-failure-hunter ─────────┤
    ├── [並列] pr-test-analyzer ──────────────┼──> 統合サマリー
    ├── [並列] type-design-analyzer ──────────┤
    ├── [並列] code-simplifier ───────────────┤
    └── [並列] comment-analyzer ──────────────┘
```

## 動作要件

- [Claude Code CLI](https://claude.ai/code)
- Git（変更ファイルの自動検出用）

## カスタマイズ

コマンドファイル（`commands/pr-review.md`）はカスタマイズ可能:

- プロジェクトの規約に合わせてエージェントプロンプトを変更
- 出力形式を調整
- プロジェクト固有のルールを追加

## コントリビューション

貢献を歓迎します！お気軽にPull Requestを送ってください。

1. リポジトリをフォーク
2. フィーチャーブランチを作成 (`git checkout -b feature/AmazingFeature`)
3. 変更をコミット (`git commit -m 'Add some AmazingFeature'`)
4. ブランチにプッシュ (`git push origin feature/AmazingFeature`)
5. Pull Requestを作成

## ライセンス

MIT License - 詳細は [LICENSE](LICENSE) を参照

## 関連プロジェクト

- [Claude Code](https://claude.ai/code) - AI搭載コーディングアシスタント
- [Anthropic Plugins](https://github.com/anthropics/claude-plugins-official) - 公式Claude Codeプラグイン

---

**Made with Claude Code**
