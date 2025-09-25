---
mode: 'agent'
description: '仕様書ファイルから未実装の要件に対して、feature_request.ymlテンプレートを使用してGitHub Issueを作成します。'
tools: ['codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
# 未対応の仕様要件に対するGitHub Issue作成

`${file}`の仕様書における未実装の要件に対してGitHub Issueを作成します。

## プロセス

1. 仕様書ファイルを分析してすべての要件を抽出する
2. 各要件のコードベース実装状況を確認する
3. 重複を避けるために`search_issues`を使用して既存のissueを検索する
4. 未実装の要件ごとに`create_issue`を使用して新しいissueを作成する
5. `feature_request.yml`テンプレートを使用する（デフォルトにフォールバック）

## 要件

- 仕様書の未実装要件ごとに1つのissue
- 明確な要件IDと説明のマッピング
- 実装ガイダンスと受け入れ基準を含める
- 作成前に既存のissueに対して検証を行う

## Issue内容

- タイトル：要件IDと簡潔な説明
- 説明：詳細な要件、実装方法、コンテキスト
- ラベル：feature、enhancement（適宜）

## 実装チェック

- 関連するコードパターンをコードベースで検索する
- `/spec/`ディレクトリの関連する仕様書ファイルを確認する
- 要件が部分的に実装されていないことを確認する