---
mode: 'agent'
description: '実装計画のフェーズから、feature_request.ymlまたはchore_request.ymlテンプレートを使用してGitHub Issueを作成します。'
tools: ['codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
# 実装計画からGitHub Issueを作成

`${file}`の実装計画に対してGitHub Issueを作成します。

## プロセス

1. 計画ファイルを分析してフェーズを特定する
2. `search_issues`を使用して既存のissueを確認する
3. `create_issue`を使用してフェーズごとに新しいissueを作成するか、`update_issue`で既存のものを更新する
4. `feature_request.yml`または`chore_request.yml`テンプレートを使用する（デフォルトにフォールバック）

## 要件

- 実装フェーズごとに1つのissue
- 明確で構造化されたタイトルと説明
- 計画で要求される変更のみを含める
- 作成前に既存のissueに対して検証を行う

## Issue内容

- タイトル：実装計画からのフェーズ名
- 説明：フェーズの詳細、要件、コンテキスト
- ラベル：issueタイプ（feature/chore）に適切なもの