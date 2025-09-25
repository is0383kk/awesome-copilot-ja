---
mode: agent
description: 'Playwright MCPを使用してシナリオに基づくPlaywrightテストを生成します'
tools: ['changes', 'codebase', 'editFiles', 'fetch', 'findTestFiles', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'playwright']
model: 'Claude Sonnet 4'
---

# Playwright MCPを使用したテスト生成

あなたの目標は、すべての規定されたステップを完了した後、提供されたシナリオに基づいてPlaywrightテストを生成することです。

## 具体的な指示

- シナリオが与えられ、そのためのPlaywrightテストを生成する必要があります。ユーザーがシナリオを提供しない場合は、シナリオの提供を求めてください。
- すべての規定されたステップを完了せずに、シナリオのみに基づいてテストコードを早々に生成しないでください。
- Playwright MCPが提供するツールを使用してステップを一つずつ実行してください。
- すべてのステップが完了した後のみ、メッセージ履歴に基づいて`@playwright/test`を使用するPlaywright TypeScriptテストを出力してください
- 生成されたテストファイルをtestsディレクトリに保存してください
- テストファイルを実行し、テストが合格するまで繰り返してください