---
description: '3つの包括的なワークフロー（基本、高度、検証）、TypeSpec サポート、Microsoft 365 Agents Toolkit 統合を備えた Microsoft 365 Copilot 宣言的エージェントの完全開発キット'
---

# Microsoft 365 宣言的エージェント開発キット

最新の v1.5 スキーマと包括的な TypeSpec および Microsoft 365 Agents Toolkit 統合を使用して、Microsoft 365 Copilot 宣言的エージェントを作成・開発することをサポートします。3つの専門的なワークフローから選択できます：

## ワークフロー 1: 基本エージェント作成
**最適な用途**: 新規開発者、シンプルなエージェント、クイックプロトタイプ

以下をガイドします：
1. **エージェント計画**: 目的、対象ユーザー、コア機能の定義
2. **機能選択**: 11の利用可能な機能から選択（WebSearch、OneDriveAndSharePoint、GraphConnectors など）
3. **基本スキーマ作成**: 適切な制約を持つ準拠した JSON マニフェストの生成
4. **TypeSpec 代替**: JSON にコンパイルされるモダンな型安全定義の作成
5. **テストセットアップ**: ローカルテスト用の Agents Playground の構成
6. **Toolkit 統合**: 強化された開発のための Microsoft 365 Agents Toolkit の活用

## ワークフロー 2: 高度な企業エージェント設計
**最適な用途**: 複雑な企業シナリオ、本番展開、高度な機能

以下をアーキテクトします：
1. **企業要件分析**: マルチテナント考慮事項、コンプライアンス、セキュリティ
2. **高度な機能構成**: 複雑な機能の組み合わせと相互作用
3. **動作オーバーライド実装**: カスタム応答パターンと専門的な動作
4. **ローカライゼーション戦略**: 適切なリソース管理による多言語サポート
5. **会話スターター**: ユーザーエンゲージメントのための戦略的な会話エントリーポイント
6. **本番展開**: 環境管理、バージョン管理、ライフサイクル計画
7. **監視・分析**: 追跡とパフォーマンス最適化の実装

## ワークフロー 3: 検証・最適化
**最適な用途**: 既存エージェント、トラブルシューティング、パフォーマンス最適化

以下を実行します：
1. **スキーマ準拠性検証**: 完全な v1.5 仕様準拠チェック
2. **文字数制限最適化**: 名前（100）、説明（1000）、指示（8000）
3. **機能監査**: 適切な機能の構成と使用の検証
4. **TypeSpec 移行**: 既存の JSON をモダンな TypeSpec 定義に変換
5. **テストプロトコル**: Agents Playground を使用した包括的な検証
6. **パフォーマンス分析**: ボトルネックと最適化機会の特定
7. **ベストプラクティスレビュー**: Microsoft ガイドラインと推奨事項との整合性

## 全ワークフロー共通のコア機能

### Microsoft 365 Agents Toolkit 統合
- **VS Code 拡張機能**: `teamsdevapp.ms-teams-vscode-extension` との完全統合
- **TypeSpec 開発**: モダンな型安全エージェント定義
- **ローカルデバッグ**: テスト用 Agents Playground 統合
- **環境管理**: 開発、ステージング、本番構成
- **ライフサイクル管理**: 作成、テスト、展開、監視

### TypeSpec の例
```typespec
// モダンな宣言的エージェント定義
model MyAgent {
  name: string;
  description: string;
  instructions: string;
  capabilities: AgentCapability[];
  conversation_starters?: ConversationStarter[];
}
```

### JSON Schema v1.5 検証
- 最新の Microsoft 仕様との完全準拠
- 文字数制限の実行（名前: 100、説明: 1000、指示: 8000）
- 配列制約検証（conversation_starters: 最大4、capabilities: 最大5）
- 必須フィールド検証と型チェック

### 利用可能な機能（最大5つまで選択）
1. **WebSearch**: インターネット検索機能
2. **OneDriveAndSharePoint**: ファイルとコンテンツアクセス
3. **GraphConnectors**: 企業データ統合
4. **MicrosoftGraph**: Microsoft 365 サービス統合
5. **TeamsAndOutlook**: コミュニケーションプラットフォームアクセス
6. **PowerPlatform**: Power Apps と Power Automate 統合
7. **BusinessDataProcessing**: 企業データ分析
8. **WordAndExcel**: ドキュメントとスプレッドシートの操作
9. **CopilotForMicrosoft365**: 高度な Copilot 機能
10. **EnterpriseApplications**: サードパーティシステム統合
11. **CustomConnectors**: カスタム API とサービス統合

### 環境変数サポート
```json
{
  "name": "${AGENT_NAME}",
  "description": "${AGENT_DESCRIPTION}",
  "instructions": "${AGENT_INSTRUCTIONS}"
}
```

**どのワークフローから始めたいですか？** 要件をお聞かせください。完全な TypeSpec と Microsoft 365 Agents Toolkit サポートを使用した Microsoft 365 Copilot 宣言的エージェント開発に特化したガイダンスを提供します。