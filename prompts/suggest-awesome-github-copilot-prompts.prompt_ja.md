---
mode: 'agent'
description: 'awesome-copilotリポジトリからの関連するGitHub Copilot promptファイルを、現在のリポジトリコンテキストとチャット履歴に基づいて提案し、このリポジトリの既存promptsとの重複を回避します。'
tools: ['changes', 'codebase', 'editFiles', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'github']
---
# Awesome GitHub Copilot Promptsの提案

現在のリポジトリコンテキストを分析し、[GitHub awesome-copilotリポジトリ](https://github.com/github/awesome-copilot/tree/main/prompts)から関連するpromptファイルで、このリポジトリにまだ存在しないものを提案します。

## プロセス

1. **利用可能なPromptsの取得**: [awesome-copilot README](https://github.com/github/awesome-copilot/blob/main/README.md)からpromptリストと説明を抽出
2. **ローカルPromptsのスキャン**: `.github/prompts/`フォルダ内の既存promptファイルを発見
3. **説明の抽出**: ローカルpromptファイルからフロントマターを読み取って説明を取得
4. **コンテキストの分析**: チャット履歴、リポジトリファイル、現在のプロジェクトニーズを確認
5. **既存のものとの比較**: このリポジトリですでに利用可能なpromptsと照らし合わせて確認
6. **関連性の一致**: 特定されたパターンと要件に対して、利用可能なpromptsを比較
7. **選択肢の提示**: 説明、根拠、可用性ステータスとともに関連promptsを表示
8. **検証**: 提案されたpromptsが既存promptsでカバーされていない価値を追加することを確認
9. **出力**: 提案、説明、awesome-copilot promptsと類似のローカルpromptsへのリンクを含む構造化テーブルを提供
10. **次のステップ**: 提案がされた場合、GitHub Copilotがpromptsディレクトリにファイルをダウンロードして提案されたpromptsをリポジトリに追加できる手順を提供。ユーザーが確認すれば自動的に実行することを提案。

## コンテキスト分析基準

🔍 **リポジトリパターン**:
- 使用されるプログラミング言語（.cs、.js、.pyなど）
- フレームワーク指標（ASP.NET、React、Azureなど）
- プロジェクトタイプ（Webアプリ、API、ライブラリ、ツール）
- ドキュメントニーズ（README、仕様、ADRなど）

🗨️ **チャット履歴コンテキスト**:
- 最近の議論と問題点
- 機能リクエストや実装ニーズ
- コードレビューパターン
- 開発ワークフロー要件

## 出力形式

awesome-copilot promptsと既存リポジトリpromptsを比較した構造化テーブルで分析結果を表示：

| Awesome-Copilot Prompt | 説明 | インストール済み | 類似のローカルPrompt | 提案根拠 |
|-------------------------|-------------|-------------------|---------------------|---------------------|
| [code-review.md](https://github.com/github/awesome-copilot/blob/main/prompts/code-review.md) | 自動コードレビューprompts | ❌ なし | なし | 標準化されたコードレビュープロセスで開発ワークフローを強化 |
| [documentation.md](https://github.com/github/awesome-copilot/blob/main/prompts/documentation.md) | プロジェクトドキュメント生成 | ✅ あり | create_oo_component_documentation.prompt.md | 既存のドキュメントpromptsでカバー済み |
| [debugging.md](https://github.com/github/awesome-copilot/blob/main/prompts/debugging.md) | デバッグ支援prompts | ❌ なし | なし | 開発チームのトラブルシューティング効率を向上 |

## ローカルPrompts発見プロセス

1. `.github/prompts/`ディレクトリ内のすべての`*.prompt.md`ファイルをリスト
2. 発見された各ファイルについて、フロントマターを読み取って`description`を抽出
3. 既存promptsの包括的なインベントリを構築
4. 重複提案を避けるためにこのインベントリを使用

## 要件

- `githubRepo`ツールを使用してawesome-copilotリポジトリからコンテンツを取得
- `.github/prompts/`ディレクトリ内の既存promptsについてローカルファイルシステムをスキャン
- ローカルpromptファイルからYAMLフロントマターを読み取って説明を抽出
- 重複を避けるためにこのリポジトリの既存promptsと比較
- 現在のpromptライブラリカバレッジのギャップに焦点を当てる
- 提案されたpromptsがリポジトリの目的と基準に一致することを検証
- 各提案について明確な根拠を提供
- awesome-copilot promptsと類似のローカルpromptsの両方へのリンクを含める
- テーブルと分析以外の追加情報やコンテキストは提供しない

## アイコン参照

- ✅ リポジトリにインストール済み
- ❌ リポジトリに未インストール