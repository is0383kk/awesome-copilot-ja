---
mode: 'agent'
description: 'feature_request.yml テンプレートを使用して仕様ファイルから機能要求の GitHub Issue を作成。'
tools: ['codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
# 仕様書から GitHub Issue を作成

`${file}` の仕様書から GitHub Issue を作成します。

## プロセス

1. 仕様ファイルを分析して要件を抽出
2. `search_issues` を使用して既存の Issue をチェック
3. `create_issue` を使用して新しい Issue を作成するか、`update_issue` で既存のものを更新
4. `feature_request.yml` テンプレートを使用（デフォルトにフォールバック）

## 要件

- 完全な仕様に対する単一の Issue
- 仕様を識別する明確なタイトル
- 仕様で要求された変更のみを含める
- 作成前に既存の Issue と照合

## Issue 内容

- タイトル: 仕様からの機能名
- 説明: 問題記述、提案された解決策、コンテキスト
- ラベル: feature、enhancement（適切な場合）