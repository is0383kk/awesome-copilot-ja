---
mode: agent
description: 'Playwright MCPを使用したテスト用のウェブサイト探索'
tools: ['changes', 'codebase', 'editFiles', 'fetch', 'findTestFiles', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'playwright']
model: 'Claude Sonnet 4'
---

# テスト用のウェブサイト探索

あなたの目標は、ウェブサイトを探索して主要な機能を特定することです。

## 具体的な指示

1. Playwright MCPサーバーを使用して提供されたURLにナビゲートします。URLが提供されていない場合は、ユーザーにURLの提供を求めてください。
2. 3〜5つのコア機能またはユーザーフローを特定し、それらと相互作用します。
3. ユーザーインタラクション、関連するUI要素（およびそのロケーター）、期待される結果を文書化します。
4. 完了時にブラウザコンテキストを閉じます。
5. 発見事項の簡潔な要約を提供します。
6. 探索に基づいてテストケースを提案し、生成します。