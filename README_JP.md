# Claude Code 並列コードレビュー

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

[English](./README.md)

**1つのコマンドで7/12/17の専門レビューを同時実行**

> コードレビューのワークフローを変革: 複数の観点から同時にフィードバックを取得し、レビュー時間を短縮しながらカバレッジを向上。

## レビューモード

| モード | コマンド | エージェント数 | 用途 |
|--------|---------|--------------|------|
| **通常** | `/pr-review` | 7 | 日常のPRレビュー、素早いチェック |
| **フル** | `/pr-review full` | 12 | 重要な機能、徹底的なレビュー |
| **リリース** | `/pr-review release` | 17 | アプリリリース、ストア申請前 |

## 特徴

- **3つのレビューモード**: 必要に応じて深さを選択
- **並列実行**: 全エージェントが同時に動作
- **セキュリティ優先**: 専用のセキュリティ脆弱性検出
- **定量的な出力**: 客観的な追跡のための数値スコア
- **優先順位付き**: Critical > Important > Suggestions
- **Git連携**: 変更ファイルを自動検出

---

## 通常モード（7エージェント）

日常のPRレビュー用。素早くコア品質をチェック。

| エージェント | 対象領域 | 出力 |
|-------------|----------|------|
| **security-analyzer** | SQLi、XSS、CSRF、認証漏れ | CRITICAL/HIGH/MEDIUM |
| **code-reviewer** | CLAUDE.md準拠、バグ | 0-100スコア |
| **silent-failure-hunter** | エラーハンドリング | CRITICAL/HIGH/MEDIUM |
| **pr-test-analyzer** | テストカバレッジの欠落 | 1-10スコア |
| **type-design-analyzer** | 型安全性、不変条件 | 4次元スコア |
| **code-simplifier** | 複雑度、リファクタリング | 具体的な提案 |
| **comment-analyzer** | コメント精度、ドキュメント | 品質レポート |

---

## フルモード（12エージェント）

包括的レビュー。重要な機能追加や徹底的なチェックに使用。

### 追加エージェント（+5）

| エージェント | 対象領域 | 出力 |
|-------------|----------|------|
| **performance-analyzer** | N+1クエリ、メモリリーク | HIGH/MEDIUM/LOW |
| **dead-code-hunter** | 未使用コード、import | リスト |
| **dependency-analyzer** | 古い/脆弱な依存 | リスクレポート |
| **accessibility-analyzer** | ARIA、コントラスト、キーボード | A11yレポート |
| **ux-analyzer** | UXアンチパターン | 提案 |

---

## リリースモード（17エージェント）

リリース前のフルチェック。App Store/Google Play審査対策を含む。

### 追加エージェント（+5）

| エージェント | 対象領域 | 出力 |
|-------------|----------|------|
| **store-review-analyzer** | Apple/Googleガイドライン | コンプライアンスレポート |
| **analytics-analyzer** | トラッキング漏れ | カバレッジレポート |
| **localization-analyzer** | 国際化、ハードコード文字列 | 問題リスト |
| **api-rate-limit-analyzer** | API制限、コスト | リスクレポート |
| **logging-analyzer** | ログ品質、漏洩 | 品質レポート |

---

## クイックスタート

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/Tenormusica2024/7-agent-parallel-code-review.git

# コマンドをClaude Code設定にコピー
cp -r 7-agent-parallel-code-review/commands/* ~/.claude/commands/
```

### 使い方

```bash
# 通常レビュー（7エージェント）
/pr-review

# フルレビュー（12エージェント）
/pr-review full

# リリースレビュー（17エージェント）
/pr-review release

# 特定ファイルをレビュー
/pr-review src/app.js src/utils.js

# 特定ディレクトリをフルレビュー
/pr-review full src/
```

## 出力例

```markdown
# PR Review Summary (Full Mode - 12 Agents)

## Critical Issues (3 found)
- [security-analyzer]: 文字列連結によるSQLインジェクション [src/db.js:23]
- [security-analyzer]: ハードコードされたAPIキーが露出 [src/config.js:5]
- [silent-failure-hunter]: 空のcatchブロックがエラーを隠蔽 [src/api.js:45]

## Important Issues (5 found)
- [performance-analyzer]: ユーザーリストでN+1クエリ [src/users.js:56]
- [type-design-analyzer]: Nullable型が処理されていない [src/user.ts:78]
- [accessibility-analyzer]: ARIAラベルがない [src/Button.tsx:12]
...

## Suggestions (8 found)
- [code-simplifier]: 繰り返しロジックをヘルパーに抽出 [src/utils.js:12-45]
- [dead-code-hunter]: 未使用import 'lodash' [src/utils.js:1]
- [ux-analyzer]: 非同期処理にローディング表示なし [src/Form.tsx:45]
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
| パフォーマンス | 1 HIGH, 2 MEDIUM |
| アクセシビリティ | 3件の問題 |
```

## 動作の仕組み

```
/pr-review [モード]
    │
    ├── [並列] security-analyzer ─────────────┐
    ├── [並列] code-reviewer ─────────────────┤
    ├── [並列] silent-failure-hunter ─────────┤
    ├── [並列] pr-test-analyzer ──────────────┤
    ├── [並列] type-design-analyzer ──────────┤
    ├── [並列] code-simplifier ───────────────┤
    ├── [並列] comment-analyzer ──────────────┤
    │                                         │
    │   [full/releaseモードのみ]              │
    ├── [並列] performance-analyzer ──────────┤
    ├── [並列] dead-code-hunter ──────────────┼──> 統合サマリー
    ├── [並列] dependency-analyzer ───────────┤
    ├── [並列] accessibility-analyzer ────────┤
    ├── [並列] ux-analyzer ───────────────────┤
    │                                         │
    │   [releaseモードのみ]                   │
    ├── [並列] store-review-analyzer ─────────┤
    ├── [並列] analytics-analyzer ────────────┤
    ├── [並列] localization-analyzer ─────────┤
    ├── [並列] api-rate-limit-analyzer ───────┤
    └── [並列] logging-analyzer ──────────────┘
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

---

**Made with Claude Code**
