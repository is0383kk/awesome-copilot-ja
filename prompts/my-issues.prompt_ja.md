---
mode: 'agent'
tools: ['githubRepo', 'github', 'get_issue', 'get_issue_comments', 'get_me', 'list_issues']
description: '現在のリポジトリで自分のイシューをリストします'
---

現在のリポジトリを検索し（リポジトリ情報には#githubRepoを使用）、自分に割り当てられているイシューを見つけて（#list_issuesを使用）リストアップしてください。

年数、コメント数、ステータス（オープン/クローズ）に基づいて、重点的に取り組むべきイシューを提案してください。