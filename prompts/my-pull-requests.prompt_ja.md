---
mode: 'agent'
tools: ['githubRepo', 'github', 'get_me', 'get_pull_request', 'get_pull_request_comments', 'get_pull_request_diff', 'get_pull_request_files', 'get_pull_request_reviews', 'get_pull_request_status', 'list_pull_requests', 'request_copilot_review']
description: '現在のリポジトリで自分のプルリクエストをリストします'
---

現在のリポジトリを検索し（リポジトリ情報には#githubRepoを使用）、自分に割り当てられているプルリクエストを見つけて（#list_pull_requestsを使用）リストアップしてください。

各プルリクエストの目的と詳細を説明してください。

PRが誰かのレビューを待っている場合は、レスポンスでそれを強調してください。

PRでチェックの失敗があった場合は、それを説明し、可能な修正を提案してください。

Copilotによるレビューがされていない場合は、#request_copilot_reviewを使用してレビューをリクエストすることを提案してください。