---
mode: 'agent'
description: '業界のベストプラクティスとアーキテクチャ文書標準に従って、オブジェクト指向コンポーネントの包括的で標準化されたドキュメントを作成します。'
tools: ['changes', 'codebase', 'editFiles', 'extensions', 'fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# 標準OOコンポーネントドキュメント生成

`${input:ComponentPath}`のオブジェクト指向コンポーネントの包括的なドキュメントを作成します。

提供されたパスのコードを分析してコンポーネントを検査します。フォルダの場合、すべてのソースファイルを分析します。単一ファイルの場合、メインコンポーネントとして扱い、同じディレクトリの関連ファイルを分析します。

## ドキュメント標準

- DOC-001: C4モデルドキュメントレベル（Context、Containers、Components、Code）に従う
- DOC-002: Arc42ソフトウェアアーキテクチャドキュメントテンプレートに合わせる
- DOC-003: IEEE 1016ソフトウェア設計記述標準に準拠する
- DOC-004: アジャイルドキュメンテーション原則を使用する（価値を追加する十分なドキュメント）
- DOC-005: 開発者と保守担当者を主要な対象者とする

## 分析指示

- ANA-001: パスタイプ（フォルダ vs 単一ファイル）を決定し、主要コンポーネントを特定する
- ANA-002: クラス構造と継承のソースコードファイルを調査する
- ANA-003: デザインパターンとアーキテクチャ決定を特定する
- ANA-004: パブリックAPI、インターフェース、依存関係をドキュメント化する
- ANA-005: 生成/構造/行動パターンを認識する
- ANA-006: メソッドパラメータ、戻り値、例外をドキュメント化する
- ANA-007: パフォーマンス、セキュリティ、信頼性、保守性を評価する
- ANA-008: 統合パターンとデータフローを推測する

## 言語固有の最適化

- LNG-001: **C#/.NET** - async/await、依存性注入、設定、破棄
- LNG-002: **Java** - Springフレームワーク、アノテーション、例外処理、パッケージング
- LNG-003: **TypeScript/JavaScript** - モジュール、非同期パターン、型、npm
- LNG-004: **Python** - パッケージ、仮想環境、型ヒント、テスト

## エラー処理

- ERR-001: パスが存在しない - 正しいフォーマットガイダンスを提供する
- ERR-002: ソースファイルが見つからない - 代替場所を提案する
- ERR-003: 不明確な構造 - 発見事項をドキュメント化し、明確化を求める
- ERR-004: 非標準パターン - カスタムアプローチをドキュメント化する
- ERR-005: 不十分なコード - 利用可能な情報に焦点を当て、ギャップを強調する

## 出力フォーマット

読みやすさと保守性のために、明確な見出し階層、コードブロック、テーブル、箇条書き、適切なフォーマットを持つ、よく構造化されたMarkdownを生成します。

## ファイル場所

ドキュメントは`/docs/components/`ディレクトリに保存し、以下の規約に従って命名する必要があります：`[component-name]-documentation.md`。

## 必須ドキュメント構造

ドキュメントファイルは以下のテンプレートに従い、すべてのセクションが適切に記入されていることを確保する必要があります。マークダウンのフロントマターは以下の例に従って正しく構造化される必要があります：

```md
---
title: [コンポーネント名] - 技術ドキュメント
component_path: `${input:ComponentPath}`
version: [オプション：例：1.0、日付]
date_created: [YYYY-MM-DD]
last_updated: [オプション：YYYY-MM-DD]
owner: [オプション：このコンポーネントに責任を持つチーム/個人]
tags: [オプション：関連するタグまたはカテゴリのリスト、例：`component`、`service`、`tool`、`infrastructure`、`documentation`、`architecture`など]
---

# [コンポーネント名] ドキュメント

[コンポーネントとシステム内でのその目的についての短い簡潔な導入。]

## 1. コンポーネント概要

### 目的/責任
- OVR-001: コンポーネントの主要な責任を述べる
- OVR-002: 範囲を定義する（含まれる/除外される機能）
- OVR-003: システムコンテキストと関係を説明する

## 2. アーキテクチャセクション

- ARC-001: 使用されるデザインパターンをドキュメント化する（Repository、Factory、Observer など）
- ARC-002: 内部および外部の依存関係を目的とともにリストする
- ARC-003: コンポーネントの相互作用と関係をドキュメント化する
- ARC-004: 視覚図を含める（UMLクラス、シーケンス、コンポーネント）
- ARC-005: コンポーネント構造、関係、依存関係を示すmermaid図を作成する

### コンポーネント構造と依存関係図

以下を示す包括的なmermaid図を含める：
- **コンポーネント構造** - メインクラス、インターフェース、およびそれらの関係
- **内部依存関係** - システム内でのコンポーネントの相互作用
- **外部依存関係** - 外部ライブラリ、サービス、データベース、API
- **データフロー** - 依存関係と相互作用の方向
- **継承/合成** - クラス階層と合成関係

```mermaid
graph TD
    subgraph "Component System"
        A[Main Component] --> B[Internal Service]
        A --> C[Internal Repository]
        B --> D[Business Logic]
        C --> E[Data Access Layer]
    end

    subgraph "External Dependencies"
        F[External API]
        G[Database]
        H[Third-party Library]
        I[Configuration Service]
    end

    A --> F
    E --> G
    B --> H
    A --> I

    classDiagram
        class MainComponent {
            +property: Type
            +method(): ReturnType
            +asyncMethod(): Promise~Type~
        }
        class InternalService {
            +businessOperation(): Result
        }
        class ExternalAPI {
            <<external>>
            +apiCall(): Data
        }

        MainComponent --> InternalService
        MainComponent --> ExternalAPI
```

## 3. インターフェースドキュメント

- INT-001: すべてのパブリックインターフェースと使用パターンをドキュメント化する
- INT-002: メソッド/プロパティ参照テーブルを作成する
- INT-003: イベント/コールバック/通知メカニズムをドキュメント化する

| メソッド/プロパティ | 目的 | パラメータ | 戻り値の型 | 使用上の注意 |
|-----------------|---------|------------|-------------|-------------|
| [名前] | [目的] | [パラメータ] | [型] | [注意事項] |

## 4. 実装詳細

- IMP-001: メインの実装クラスと責任をドキュメント化する
- IMP-002: 設定要件と初期化を説明する
- IMP-003: 主要アルゴリズムとビジネスロジックをドキュメント化する
- IMP-004: パフォーマンス特性とボトルネックを記録する

## 5. 使用例

### 基本使用法

```csharp
// 基本使用例
var component = new ComponentName();
component.DoSomething();
```

### 高度な使用法

```csharp
// 高度な設定パターン
var options = new ComponentOptions();
var component = ComponentFactory.Create(options);
await component.ProcessAsync(data);
```

- USE-001: 基本使用例を提供する
- USE-002: 高度な設定パターンを示す
- USE-003: ベストプラクティスと推奨パターンをドキュメント化する

## 6. 品質属性

- QUA-001: セキュリティ（認証、認可、データ保護）
- QUA-002: パフォーマンス（特性、スケーラビリティ、リソース使用）
- QUA-003: 信頼性（エラー処理、障害許容性、復旧）
- QUA-004: 保守性（標準、テスト、ドキュメント）
- QUA-005: 拡張性（拡張ポイント、カスタマイズオプション）

## 7. 参考情報

- REF-001: バージョンと目的を持つ依存関係をリストする
- REF-002: 完全な設定オプション参考資料
- REF-003: テストガイドラインとモックセットアップ
- REF-004: トラブルシューティング（一般的な問題、エラーメッセージ）
- REF-005: 関連ドキュメントリンク
- REF-006: 変更履歴と移行ノート

```