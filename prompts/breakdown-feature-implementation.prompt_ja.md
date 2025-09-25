---
mode: 'agent'
description: 'Epochモノレポ構造に従った詳細な機能実装計画を作成するプロンプト。'
---

# 機能実装計画プロンプト

## 目標

大規模SaaS企業向けの高品質機能を作成する責任を持つ業界ベテランのソフトウェアエンジニアとして行動してください。機能PRDに基づく詳細な技術実装計画の作成に長けています。
提供されたコンテキストを確認し、徹底的で包括的な実装計画を出力してください。
**注意：** 技術的状況における疑似コードでない限り、出力にコードを書かないでください。

## 出力形式

出力は、`/docs/ways-of-work/plan/{epic-name}/{feature-name}/implementation-plan.md`に保存される、Markdown形式での完全な実装計画である必要があります。

### ファイルシステム

Epochのモノレポ構造に従ったフロントエンドとバックエンドリポジトリ両方のフォルダーとファイル構造：

```
apps/
  [app-name]/
services/
  [service-name]/
packages/
  [package-name]/
```

### 実装計画

各機能について：

#### 目標

機能目標の説明（3-5文）

#### 要件

- 詳細な機能要件（箇条書きリスト）
- 実装計画の詳細

#### 技術的考慮事項

##### システムアーキテクチャ概要

この機能が全体システムにどのように統合されるかを示す包括的なシステムアーキテクチャ図をMermaidを使用して作成してください。図には以下を含める必要があります：

- **フロントエンド層**：ユーザーインターフェースコンポーネント、状態管理、クライアントサイドロジック
- **API層**：tRPCエンドポイント、認証ミドルウェア、入力検証、リクエストルーティング
- **ビジネスロジック層**：サービスクラス、ビジネスルール、ワークフローオーケストレーション、イベント処理
- **データ層**：データベース相互作用、キャッシュメカニズム、外部API統合
- **インフラストラクチャ層**：Dockerコンテナ、バックグラウンドサービス、デプロイメントコンポーネント

これらの層を明確に整理するためにサブグラフを使用してください。リクエスト/レスポンスパターン、データ変換、イベントフローを示すラベル付き矢印で層間のデータフローを示してください。この実装に固有の機能特有のコンポーネント、サービス、またはデータ構造を含めてください。

- **技術スタック選択**：各層の選択根拠を文書化
```

- **技術スタック選択**：各層の選択根拠を文書化
- **統合ポイント**：明確な境界と通信プロトコルの定義
- **デプロイメントアーキテクチャ**：Dockerコンテナ化戦略
- **スケーラビリティ考慮事項**：水平および垂直スケーリングアプローチ

##### データベーススキーマ設計

機能のデータモデルを示すMermaidを使用したエンティティ関係図を作成：

- **テーブル仕様**：型と制約を含む詳細なフィールド定義
- **インデックス戦略**：パフォーマンス重要なインデックスとその根拠
- **外部キー関係**：データ整合性と参照制約
- **データベースマイグレーション戦略**：バージョン管理とデプロイメントアプローチ

##### API設計

- 完全な仕様を持つエンドポイント
- TypeScript型でのリクエスト/レスポンス形式
- Stack Authでの認証と認可
- エラーハンドリング戦略とステータスコード
- レート制限とキャッシュ戦略

##### フロントエンドアーキテクチャ

###### コンポーネント階層文書

コンポーネント構造は、一貫性とアクセシビリティの基盤として`shadcn/ui`ライブラリを活用します。

**レイアウト構造：**

```
Recipe Library Page
├── Header Section (shadcn: Card)
│   ├── Title (shadcn: Typography `h1`)
│   ├── Add Recipe Button (shadcn: Button with DropdownMenu)
│   │   ├── Manual Entry (DropdownMenuItem)
│   │   ├── Import from URL (DropdownMenuItem)
│   │   └── Import from PDF (DropdownMenuItem)
│   └── Search Input (shadcn: Input with icon)
├── Main Content Area (flex container)
│   ├── Filter Sidebar (aside)
│   │   ├── Filter Title (shadcn: Typography `h4`)
│   │   ├── Category Filters (shadcn: Checkbox group)
│   │   ├── Cuisine Filters (shadcn: Checkbox group)
│   │   └── Difficulty Filters (shadcn: RadioGroup)
│   └── Recipe Grid (main)
│       └── Recipe Card (shadcn: Card)
│           ├── Recipe Image (img)
│           ├── Recipe Title (shadcn: Typography `h3`)
│           ├── Recipe Tags (shadcn: Badge)
│           └── Quick Actions (shadcn: Button - View, Edit)
```

- **状態フロー図**：Mermaidを使用したコンポーネント状態管理
- 再利用可能なコンポーネントライブラリ仕様
- Zustand/React Queryでの状態管理パターン
- TypeScriptインターフェースと型

##### セキュリティパフォーマンス

- 認証/認可要件
- データ検証とサニタイゼーション
- パフォーマンス最適化戦略
- キャッシュメカニズム

## コンテキストテンプレート

- **機能PRD：** [機能PRDマークダウンファイルの内容]