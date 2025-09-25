---
description: 'nvie Git Flow ブランチングモデルに従って、git status/diff を分析し適切なブランチを作成するインテリジェントな Git Flow ブランチ作成ツール。'
tools: ['run_in_terminal', 'get_terminal_output']
mode: 'agent'
---

### 指示

```xml
<instructions>
	<title>Git Flow ブランチ作成ツール</title>
	<description>このプロンプトは git status と git diff（または git diff --cached）を使用して現在の git 変更を分析し、Git Flow ブランチングモデルに従って適切なブランチタイプをインテリジェントに決定し、セマンティックなブランチ名を作成します。</description>
	<note>
		このプロンプトを実行するだけで、Copilot があなたの変更を分析し、適切な Git Flow ブランチを作成してくれます。
	</note>
</instructions>
```

### ワークフロー

**以下の手順に従ってください：**

1. `git status` を実行して現在のリポジトリの状態と変更されたファイルをレビューします。
2. `git diff`（ステージされていない変更用）または `git diff --cached`（ステージされた変更用）を実行して変更の性質を分析します。
3. 下記の Git Flow ブランチ分析フレームワークを使用して変更を分析します。
4. 分析に基づいて適切なブランチタイプを決定します。
5. Git Flow 規則に従ってセマンティックなブランチ名を生成します。
6. ブランチを自動的に作成して切り替えます。
7. 分析の概要と次のステップを提供します。

### Git Flow ブランチ分析フレームワーク

```xml
<analysis-framework>
	<branch-types>
		<feature>
			<purpose>新機能、拡張、非重要な改善</purpose>
			<branch-from>develop</branch-from>
			<merge-to>develop</merge-to>
			<naming>feature/descriptive-name または feature/ticket-number-description</naming>
			<indicators>
				<indicator>新しい機能の追加</indicator>
				<indicator>UI/UX の改善</indicator>
				<indicator>新しい API エンドポイントまたはメソッド</indicator>
				<indicator>データベーススキーマの追加（非破壊的）</indicator>
				<indicator>新しい設定オプション</indicator>
				<indicator>パフォーマンス改善（非重要）</indicator>
			</indicators>
		</feature>

		<release>
			<purpose>リリース準備、バージョン更新、最終テスト</purpose>
			<branch-from>develop</branch-from>
			<merge-to>develop AND master</merge-to>
			<naming>release-X.Y.Z</naming>
			<indicators>
				<indicator>バージョン番号の変更</indicator>
				<indicator>ビルド設定の更新</indicator>
				<indicator>ドキュメントの最終化</indicator>
				<indicator>リリース前のマイナーなバグ修正</indicator>
				<indicator>リリースノートの更新</indicator>
				<indicator>依存関係バージョンのロック</indicator>
			</indicators>
		</release>

		<hotfix>
			<purpose>即座のデプロイが必要な重要な本番バグ修正</purpose>
			<branch-from>master</branch-from>
			<merge-to>develop AND master</merge-to>
			<naming>hotfix-X.Y.Z または hotfix/critical-issue-description</naming>
			<indicators>
				<indicator>セキュリティ脆弱性の修正</indicator>
				<indicator>重要な本番バグ</indicator>
				<indicator>データ破損の修正</indicator>
				<indicator>サービス停止の解決</indicator>
				<indicator>緊急設定変更</indicator>
			</indicators>
		</hotfix>
	</branch-types>
</analysis-framework>
```

### ブランチ命名規則

```xml
<naming-conventions>
	<feature-branches>
		<format>feature/[ticket-number-]descriptive-name</format>
		<examples>
			<example>feature/user-authentication</example>
			<example>feature/PROJ-123-shopping-cart</example>
			<example>feature/api-rate-limiting</example>
			<example>feature/dashboard-redesign</example>
		</examples>
	</feature-branches>

	<release-branches>
		<format>release-X.Y.Z</format>
		<examples>
			<example>release-1.2.0</example>
			<example>release-2.1.0</example>
			<example>release-1.0.0</example>
		</examples>
	</release-branches>

	<hotfix-branches>
		<format>hotfix-X.Y.Z OR hotfix/critical-description</format>
		<examples>
			<example>hotfix-1.2.1</example>
			<example>hotfix/security-patch</example>
			<example>hotfix/payment-gateway-fix</example>
			<example>hotfix-2.1.1</example>
		</examples>
	</hotfix-branches>
</naming-conventions>
```

### 分析プロセス

```xml
<analysis-process>
	<step-1>
		<title>変更性質の分析</title>
		<description>変更されたファイルのタイプと変更の性質を調査</description>
		<criteria>
			<files-modified>ファイル拡張子、ディレクトリ構造、目的を確認</files-modified>
			<change-scope>変更が追加的、修正的、または準備的かを判断</change-scope>
			<urgency-level>変更が重要な問題に対処するか開発的かを評価</urgency-level>
		</criteria>
	</step-1>

	<step-2>
		<title>Git Flow 分類</title>
		<description>変更を適切な Git Flow ブランチタイプにマッピング</description>
		<decision-tree>
			<question>これらは本番問題の重要な修正ですか？</question>
			<if-yes>hotfix ブランチを検討</if-yes>
			<if-no>
				<question>これらはリリース準備の変更（バージョン更新、最終調整）ですか？</question>
				<if-yes>release ブランチを検討</if-yes>
				<if-no>デフォルトで feature ブランチ</if-no>
			</if-no>
		</decision-tree>
	</step-2>

	<step-3>
		<title>ブランチ名生成</title>
		<description>セマンティックで説明的なブランチ名を作成</description>
		<guidelines>
			<use-kebab-case>ハイフン付きの小文字を使用</use-kebab-case>
			<be-descriptive>名前は目的を明確に示すべき</be-descriptive>
			<include-context>利用可能な場合はチケット番号やプロジェクトコンテキストを追加</include-context>
			<keep-concise>過度に長い名前は避ける</keep-concise>
		</guidelines>
	</step-3>
</analysis-process>
```

### エッジケースと検証

```xml
<edge-cases>
	<mixed-changes>
		<scenario>変更に機能とバグ修正の両方が含まれる</scenario>
		<resolution>最も重要な変更タイプを優先するか、複数のブランチに分割することを提案</resolution>
	</mixed-changes>

	<no-changes>
		<scenario>git status/diff で変更が検出されない</scenario>
		<resolution>ユーザーに通知し、最初に git status をチェックするか変更を行うことを提案</resolution>
	</no-changes>

	<existing-branch>
		<scenario>すでに feature/hotfix/release ブランチにいる</scenario>
		<resolution>新しいブランチが必要か、現在のブランチが適切かを分析</resolution>
	</existing-branch>

	<conflicting-names>
		<scenario>提案されたブランチ名がすでに存在する</scenario>
		<resolution>増分サフィックスを追加するか代替名を提案</resolution>
	</conflicting-names>
</edge-cases>
```

### 例

```xml
<examples>
	<example-1>
		<scenario>新しいユーザー登録 API エンドポイントを追加</scenario>
		<analysis>新機能、追加的変更、非重要</analysis>
		<branch-type>feature</branch-type>
		<branch-name>feature/user-registration-api</branch-name>
		<command>git checkout -b feature/user-registration-api develop</command>
	</example-1>

	<example-2>
		<scenario>認証における重要なセキュリティ脆弱性を修正</scenario>
		<analysis>セキュリティ修正、本番にとって重要、即座のデプロイが必要</analysis>
		<branch-type>hotfix</branch-type>
		<branch-name>hotfix/auth-security-patch</branch-name>
		<command>git checkout -b hotfix/auth-security-patch master</command>
	</example-2>

	<example-3>
		<scenario>バージョンを 2.1.0 に更新しリリースノートを最終化</scenario>
		<analysis>リリース準備、バージョン更新、ドキュメント</analysis>
		<branch-type>release</branch-type>
		<branch-name>release-2.1.0</branch-name>
		<command>git checkout -b release-2.1.0 develop</command>
	</example-3>

	<example-4>
		<scenario>データベースクエリのパフォーマンスを改善しキャッシュを更新</scenario>
		<analysis>パフォーマンス改善、非重要な拡張</analysis>
		<branch-type>feature</branch-type>
		<branch-name>feature/database-performance-optimization</branch-name>
		<command>git checkout -b feature/database-performance-optimization develop</command>
	</example-4>
</examples>
```

### 検証チェックリスト

```xml
<validation>
	<pre-analysis>
		<check>リポジトリがクリーンな状態にある（競合するコミットされていない変更がない）</check>
		<check>現在のブランチが適切な開始ポイント（feature/release は develop、hotfix は master）</check>
		<check>リモートリポジトリが最新</check>
	</pre-analysis>

	<analysis-quality>
		<check>変更分析がすべての変更されたファイルをカバー</check>
		<check>ブランチタイプ選択が Git Flow 原則に従う</check>
		<check>ブランチ名がセマンティックで規則に従う</check>
		<check>エッジケースが考慮され処理される</check>
	</analysis-quality>

	<execution-safety>
		<check>ターゲットブランチ（develop/master）が存在しアクセス可能</check>
		<check>提案されたブランチ名が既存のブランチと競合しない</check>
		<check>ユーザーがブランチ作成の適切な権限を持つ</check>
	</execution-safety>
</validation>
```

### 最終実行

```xml
<execution-protocol>
	<analysis-summary>
		<git-status>git status コマンドの出力</git-status>
		<git-diff>git diff 出力の関連部分</git-diff>
		<change-analysis>変更が何を表すかの詳細分析</change-analysis>
		<branch-decision>特定のブランチタイプが選ばれた理由の説明</branch-decision>
	</analysis-summary>

	<branch-creation>
		<command>git checkout -b [branch-name] [source-branch]</command>
		<confirmation>ブランチ作成と現在のブランチ状態を検証</confirmation>
		<next-steps>次のアクション（変更をコミット、ブランチをプッシュなど）のガイダンスを提供</next-steps>
	</branch-creation>

	<fallback-options>
		<alternative-names>主要提案が適切でない場合の 2-3 の代替ブランチ名を提案</alternative-names>
		<manual-override>分析が間違っていると思われる場合にユーザーが異なるブランチタイプを指定できるようにする</manual-override>
	</fallback-options>
</execution-protocol>
```

### Git Flow リファレンス

```xml
<gitflow-reference>
	<main-branches>
		<master>本番可能なコード、すべてのコミットがリリース</master>
		<develop>機能の統合ブランチ、最新の開発変更</develop>
	</main-branches>

	<supporting-branches>
		<feature>develop からブランチ、develop にマージバック</feature>
		<release>develop からブランチ、develop と master の両方にマージ</release>
		<hotfix>master からブランチ、develop と master の両方にマージ</hotfix>
	</supporting-branches>

	<merge-strategy>
		<flag>ブランチ履歴を保持するため常に --no-ff フラグを使用</flag>
		<tagging>master ブランチでリリースをタグ付け</tagging>
		<cleanup>成功したマージ後にブランチを削除</cleanup>
	</merge-strategy>
</gitflow-reference>
```