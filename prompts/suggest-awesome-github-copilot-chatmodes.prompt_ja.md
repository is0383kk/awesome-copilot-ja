---
mode: 'agent'
description: 'awesome-copilotリポジトリからの関連するGitHub Copilot chatmodeファイルを、現在のリポジトリコンテキストとチャット履歴に基づいて提案し、このリポジトリの既存chatmodesとの重複を回避します。'
tools: ['changes', 'codebase', 'editFiles', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'github']
---

# Awesome GitHub Copilot Chatmodesの提案

現在のリポジトリコンテキストを分析し、[GitHub awesome-copilotリポジトリ](https://github.com/github/awesome-copilot/tree/main/chatmodes)から関連するchatmodeファイルで、このリポジトリにまだ存在しないものを提案します。

## プロセス

1. **利用可能なChatmodesの取得**: [awesome-copilot chatmodesフォルダ](https://github.com/github/awesome-copilot/tree/main/chatmodes)からchatmodeリストと説明を抽出
2. **ローカルChatmodesのスキャン**: `.github/chatmodes/`フォルダ内の既存chatmodeファイルを発見
3. **説明の抽出**: ローカルchatmodeファイルからフロントマターを読み取って説明を取得
4. **コンテキストの分析**: チャット履歴、リポジトリファイル、現在のプロジェクトニーズを確認
5. **既存のものとの比較**: このリポジトリですでに利用可能なchatmodesと照らし合わせて確認
6. **関連性の一致**: 特定されたパターンと要件に対して、利用可能なchatmodesを比較
7. **選択肢の提示**: 説明、根拠、可用性ステータスとともに関連chatmodesを表示
8. **検証**: 提案されたchatmodesが既存chatmodesでカバーされていない価値を追加することを確認
9. **出力**: 提案、説明、awesome-copilot chatmodesと類似のローカルchatmodesへのリンクを含む構造化テーブルを提供
10. **次のステップ**: 提案がされた場合、GitHub Copilotがchatmodeディレクトリにファイルをダウンロードして提案されたchatmodesをリポジトリに追加できる手順を提供。ユーザーが確認すれば自動的に実行することを提案。

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

awesome-copilot chatmodesと既存リポジトリchatmodesを比較した構造化テーブルで分析結果を表示：

| Awesome-Copilot Chatmode | 説明 | インストール済み | 類似のローカルChatmode | 提案根拠 |
|---------------------------|-------------|-------------------|-------------------------|---------------------|
| [code-reviewer.chatmode.md](https://github.com/github/awesome-copilot/blob/main/chatmodes/code-reviewer.chatmode.md) | 専用コードレビューchatmode | ❌ なし | なし | 専用コードレビュー支援で開発ワークフローを強化 |
| [architect.chatmode.md](https://github.com/github/awesome-copilot/blob/main/chatmodes/architect.chatmode.md) | ソフトウェアアーキテクチャガイダンス | ✅ あり | azure_principal_architect.chatmode.md | 既存のアーキテクチャchatmodesでカバー済み |
| [debugging-expert.chatmode.md](https://github.com/github/awesome-copilot/blob/main/chatmodes/debugging-expert.chatmode.md) | デバッグ支援chatmode | ❌ なし | なし | 開発チームのトラブルシューティング効率を向上 |

## ローカルChatmodes発見プロセス

1. `.github/chatmodes/`ディレクトリ内のすべての`*.chatmode.md`ファイルをリスト
2. 発見された各ファイルについて、フロントマターを読み取って`description`を抽出
3. 既存chatmodesの包括的なインベントリを構築
4. 重複提案を避けるためにこのインベントリを使用

## 要件

- `githubRepo`ツールを使用してawesome-copilotリポジトリchatmodesフォルダからコンテンツを取得
- `.github/chatmodes/`ディレクトリ内の既存chatmodesについてローカルファイルシステムをスキャン
- ローカルchatmodeファイルからYAMLフロントマターを読み取って説明を抽出
- 重複を避けるためにこのリポジトリの既存chatmodesと比較
- 現在のchatmodeライブラリカバレッジのギャップに焦点を当てる
- 提案されたchatmodesがリポジトリの目的と基準に一致することを検証
- 各提案について明確な根拠を提供
- awesome-copilot chatmodesと類似のローカルchatmodesの両方へのリンクを含める
- テーブルと分析以外の追加情報やコンテキストは提供しない

## アイコン参照

- ✅ リポジトリにインストール済み
- ❌ リポジトリに未インストール