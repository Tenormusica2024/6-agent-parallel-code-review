---
description: "並列コードレビュー実行（7/12/17エージェント）"
argument-hint: "[full|release] [対象ファイル/ディレクトリ]"
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Task"]
---

# PR Review - 並列コードレビュー

**引数:** "$ARGUMENTS"

## モード選択

引数に応じて実行するエージェント数が変わります:

- **引数なし / 通常**: 7エージェント（日常用・軽量）
- **full**: 12エージェント（包括的レビュー）
- **release**: 17エージェント（アプリリリース用フルチェック）

---

## 通常モード（7エージェント）

日常のPRレビュー用。素早くコア品質をチェック。

### エージェント一覧

1. **security-analyzer** - セキュリティ脆弱性検出
   - SQLインジェクション、XSS、CSRF検出
   - 認証・認可漏れ、シークレット露出検出

2. **code-reviewer** - 一般的コード品質（0-100スコア）
   - CLAUDE.md準拠チェック
   - バグ・問題検出

3. **silent-failure-hunter** - エラーハンドリング漏れ検出
   - サイレント障害検出
   - catch/tryブロックレビュー

4. **pr-test-analyzer** - テストカバレッジ（1-10スコア）
   - テストカバレッジ評価
   - 重要なギャップ特定

5. **type-design-analyzer** - 型設計・不変条件（4次元評価）
   - 型カプセル化分析
   - 不変条件の適切な表現確認

6. **code-simplifier** - 複雑度削減・簡素化提案
   - 複雑なコードの簡素化
   - 可読性向上提案

7. **comment-analyzer** - コメント精度・ドキュメント品質
   - コメントとコードの整合性検証
   - ドキュメント完全性チェック

### 実行コマンド（7並列）
```
Task(subagent_type="code-reviewer", prompt="security-analyzer: [ファイルリスト]のセキュリティ脆弱性を検出。SQLi/XSS/CSRF/認証漏れ/シークレット露出を重大度CRITICAL/HIGH/MEDIUMで分類")
Task(subagent_type="code-reviewer", prompt="[ファイルリスト]をレビュー。CLAUDE.md準拠・バグ検出・0-100スコア")
Task(subagent_type="code-reviewer", prompt="silent-failure-hunter: [ファイルリスト]のサイレント障害・catch漏れ・エラーログ品質を検出。重大度CRITICAL/HIGH/MEDIUMで分類")
Task(subagent_type="code-reviewer", prompt="pr-test-analyzer: [ファイルリスト]のテストカバレッジを1-10スコアで評価")
Task(subagent_type="code-reviewer", prompt="type-design-analyzer: [ファイルリスト]の型正確性・不変条件・NULL安全性・データフローを各1-10で4次元評価")
Task(subagent_type="code-reviewer", prompt="code-simplifier: [ファイルリスト]の複雑度削減・重複コード・リファクタリング提案。具体的コード改善例を含める")
Task(subagent_type="code-reviewer", prompt="comment-analyzer: [ファイルリスト]のコメント精度・ドキュメント完全性を評価")
```

---

## fullモード（12エージェント）

包括的レビュー。重要な機能追加や品質をしっかり見たい時に使用。

### 追加エージェント（8-12）

8. **performance-analyzer** - パフォーマンス問題検出
   - N+1クエリ、メモリリーク検出
   - 非効率なループ・アルゴリズム検出

9. **dead-code-hunter** - 未使用コード検出
   - 未使用変数・関数・import検出
   - 到達不能コード検出

10. **dependency-analyzer** - 依存関係分析
    - 古い/脆弱な依存パッケージ検出
    - 循環依存検出

11. **accessibility-analyzer** - アクセシビリティ検出
    - ARIA属性の適切な使用
    - コントラスト比、キーボード操作対応

12. **ux-analyzer** - UXアンチパターン検出
    - 無限ローディング、フィードバックなしボタン
    - 確認なし削除、長すぎるフォーム

### 追加実行コマンド（+5並列）
```
Task(subagent_type="code-reviewer", prompt="performance-analyzer: [ファイルリスト]のパフォーマンス問題を検出。N+1クエリ/メモリリーク/非効率ループ/重い計算を重大度HIGH/MEDIUM/LOWで分類")
Task(subagent_type="code-reviewer", prompt="dead-code-hunter: [ファイルリスト]の未使用コードを検出。未使用変数/関数/import/到達不能コードをリスト化")
Task(subagent_type="code-reviewer", prompt="dependency-analyzer: [ファイルリスト]の依存関係を分析。古いパッケージ/脆弱性のある依存/循環依存を検出")
Task(subagent_type="code-reviewer", prompt="accessibility-analyzer: [ファイルリスト]のアクセシビリティを検証。ARIA属性/コントラスト比/キーボード操作/スクリーンリーダー対応を評価")
Task(subagent_type="code-reviewer", prompt="ux-analyzer: [ファイルリスト]のUXアンチパターンを検出。無限ローディング/フィードバック欠如/確認なし削除/長いフォーム/分かりにくいエラー表示を指摘")
```

---

## releaseモード（17エージェント）

アプリリリース前のフルチェック。App Store/Google Play審査対策を含む。

### 追加エージェント（13-17）

13. **store-review-analyzer** - ストア審査対策
    - Apple/Google審査ガイドライン準拠
    - プライバシー要件、課金ルール確認

14. **analytics-analyzer** - トラッキング漏れ検出
    - 重要イベントのトラッキング漏れ
    - 効果測定に必要なデータ収集確認

15. **localization-analyzer** - 国際化対応
    - ハードコードされた文字列検出
    - 日付・通貨・数値フォーマット確認

16. **api-rate-limit-analyzer** - API制限・コスト分析
    - 外部API呼び出しの制限確認
    - コスト爆発リスクの検出

17. **logging-analyzer** - ログ品質
    - 本番環境での適切なログレベル
    - センシティブ情報のログ出力検出

### 追加実行コマンド（+5並列）
```
Task(subagent_type="code-reviewer", prompt="store-review-analyzer: [ファイルリスト]のApp Store/Google Play審査対策を確認。プライバシー要件/課金ルール/禁止コンテンツ/技術要件を検証")
Task(subagent_type="code-reviewer", prompt="analytics-analyzer: [ファイルリスト]のトラッキング実装を確認。重要イベント/コンバージョン/ユーザー行動の計測漏れを検出")
Task(subagent_type="code-reviewer", prompt="localization-analyzer: [ファイルリスト]の国際化対応を確認。ハードコード文字列/日付通貨フォーマット/RTL対応を検証")
Task(subagent_type="code-reviewer", prompt="api-rate-limit-analyzer: [ファイルリスト]のAPI使用を分析。レート制限/コスト/クォータ超過リスクを検出")
Task(subagent_type="code-reviewer", prompt="logging-analyzer: [ファイルリスト]のログ実装を確認。適切なログレベル/センシティブ情報露出/デバッグログ残りを検出")
```

---

## 実行手順

1. まず対象ファイルを特定:
   - 引数でファイル/ディレクトリ指定があればそれを使用
   - なければ `git diff --name-only` で変更ファイルを取得

2. モードに応じたTaskツールを並列実行

3. 結果を以下の形式で集約:

```markdown
# PR Review Summary (モード名)

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
| セキュリティ | CRITICAL/HIGH/MEDIUM件数 |
| コード品質 | X/100 |
| テストカバレッジ | X/10 |
| 型設計 | 4次元評価結果 |
| パフォーマンス | (fullモード以上) |
| アクセシビリティ | (fullモード以上) |
```

## 重要

- **モードに応じた数のTaskを並列実行すること**
- 結果が返ってきたら統合サマリーを作成
- Critical > Important > Suggestions の優先度で報告
- **セキュリティ問題は最優先で報告**
