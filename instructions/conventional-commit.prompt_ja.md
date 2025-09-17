---
description: '構造化されたXML形式を使用してコンベンショナルコミットメッセージを生成するためのプロンプトとワークフロー。ユーザーがコンベンショナルコミット仕様に準拠した標準化された説明的なコミットメッセージを作成するための指示、例、検証を提供します。'
tools: ['run_in_terminal', 'get_terminal_output']
---

### 指示

```xml
	<description>このファイルは、コンベンショナルコミットメッセージを生成するためのプロンプトテンプレートを含んでいます。コンベンショナルコミット仕様に準拠した標準化された説明的なコミットメッセージをユーザーが書くためのの指示、例、フォーマットガイドラインを提供します。</description>
	<note>
```

### ワークフロー

**以下の手順に従ってください：**

1. `git status`を実行して変更されたファイルを確認する。
2. `git diff`または`git diff --cached`を実行して変更を検査する。
3. `git add <file>`で変更をステージングする。
4. 以下のXML構造を使用してコミットメッセージを構築する。
5. コミットメッセージを生成した後、Copilotが統合ターミナルで以下のコマンドを自動的に実行します（確認は不要）：

```bash
git commit -m "type(scope): description"
```

6. このプロンプトを実行するだけで、Copilotがターミナルでコミットを処理します。

### コミットメッセージ構造

```xml
<commit-message>
	<type>feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert</type>
	<scope>()</scope>
	<description>変更の短い命令的な要約</description>
	<body>(オプション: より詳細な説明)</body>
	<footer>(オプション: 例: BREAKING CHANGE: 詳細、または課題参照)</footer>
</commit-message>
```

### 例

```xml
<examples>
	<example>feat(parser): 配列を解析する機能を追加</example>
	<example>fix(ui): ボタンの配置を修正</example>
	<example>docs: 使用方法のREADMEを更新</example>
	<example>refactor: データ処理のパフォーマンスを改善</example>
	<example>chore: 依存関係を更新</example>
	<example>feat!: 登録時にメール送信（BREAKING CHANGE: メールサービスが必要）</example>
</examples>
```

### 検証

```xml
<validation>
	<type>許可されたタイプのいずれかである必要があります。参照：<reference>https://www.conventionalcommits.org/en/v1.0.0/#specification</reference></type>
	<scope>オプションですが、明確性のため推奨されます。</scope>
	<description>必須。命令法を使用してください（例：「追加」、「追加された」ではなく）。</description>
	<body>オプション。追加コンテキストに使用。</body>
	<footer>破壊的変更や課題参照に使用。</footer>
</validation>
```

### 最終ステップ

```xml
<final-step>
	<cmd>git commit -m "type(scope): description"</cmd>
	<note>構築したメッセージに置き換えてください。必要に応じてbodyとfooterを含めます。</note>
</final-step>
```
