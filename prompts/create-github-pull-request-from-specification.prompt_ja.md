---
mode: 'agent'
description: '仕様書ファイルからpull_request_template.mdテンプレートを使用してGitHub Pull Requestを作成します。'
tools: ['codebase', 'search', 'github', 'create_pull_request', 'update_pull_request', 'get_pull_request_diff']
---
# 仕様書からGitHub Pull Requestを作成

`${workspaceFolder}/.github/pull_request_template.md`の仕様書に対してGitHub Pull Requestを作成します。

## プロセス

1. `search`ツールを使用して`${workspaceFolder}/.github/pull_request_template.md`から仕様書ファイルテンプレートを分析し、要件を抽出する。
2. `create_pull_request`ツールを使用して`${input:targetBranch}`にプルリクエストのドラフトテンプレートを作成する。`get_pull_request`で現在のブランチの既存のプルリクエストがないことを確認する。ある場合はステップ4に進み、ステップ3をスキップする。
3. `get_pull_request_diff`ツールを使用してプルリクエストの変更を取得し、Pull Requestで変更された情報を分析する。
4. `update_pull_request`ツールを使用して前のステップで作成されたプルリクエストのボディとタイトルを更新する。最初のステップで取得したテンプレートの情報を組み込んで、必要に応じてボディとタイトルを更新する。
5. `update_pull_request`ツールを使用してドラフトからレビュー準備完了に切り替える。プルリクエストの状態を更新する。
6. `get_me`を使用してプルリクエストを作成した人のユーザー名を取得し、`update_issue`ツールに割り当てる。プルリクエストを割り当てる。
7. 作成されたプルリクエストのURLをユーザーに応答する。

## 要件
- 完全な仕様に対する単一のプルリクエスト
- 仕様を識別する明確なタイトル/pull_request_template.md
- pull_request_template.mdに十分な情報を記入する
- 作成前に既存のプルリクエストに対して検証を行う