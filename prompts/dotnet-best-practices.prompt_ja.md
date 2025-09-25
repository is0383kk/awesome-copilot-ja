---
mode: 'agent'
description: '.NET/C# コードがソリューション/プロジェクトのベストプラクティスに適合していることを確認する。'
---
# .NET/C# ベストプラクティス

${selection} 内の .NET/C# コードがこのソリューション/プロジェクトに特有のベストプラクティスに適合していることを確認することがあなたのタスクです。これには以下が含まれます：

## ドキュメントと構造

- すべてのパブリッククラス、インターフェース、メソッド、プロパティに対して包括的な XML ドキュメントコメントを作成する
- XML コメントにパラメーター説明と戻り値説明を含める
- 確立された名前空間構造に従う：{Core|Console|App|Service}.{Feature}

## デザインパターンとアーキテクチャ

- 依存性注入にプライマリコンストラクター構文を使用する（例：`public class MyClass(IDependency dependency)`）
- ジェネリック基底クラスでコマンドハンドラーパターンを実装する（例：`CommandHandler<TOptions>`）
- 明確な命名規則でインターフェース分離を使用する（インターフェースに 'I' のプレフィックスを付ける）
- 複雑なオブジェクト生成にファクトリーパターンに従う

## 依存性注入とサービス

- ArgumentNullException による null チェックでコンストラクター依存性注入を使用する
- 適切なライフタイム（Singleton、Scoped、Transient）でサービスを登録する
- Microsoft.Extensions.DependencyInjection パターンを使用する
- テスト可能性のためのサービスインターフェースを実装する

## リソース管理とローカライゼーション

- ローカライズされたメッセージとエラー文字列に ResourceManager を使用する
- LogMessages と ErrorMessages リソースファイルを分離する
- `_resourceManager.GetString("MessageKey")` でリソースにアクセスする

## Async/Await パターン

- すべての I/O 操作と長時間実行タスクに async/await を使用する
- 非同期メソッドから Task または Task<T> を返す
- 適切な場合に ConfigureAwait(false) を使用する
- 非同期例外を適切に処理する

## テスト標準

- アサーションに FluentAssertions を使用した MSTest フレームワークを使用する
- AAA パターン（Arrange、Act、Assert）に従う
- 依存関係のモック化に Moq を使用する
- 成功と失敗の両方のシナリオをテストする
- null パラメーター検証テストを含める

## 構成と設定

- データアノテーション付きの強く型付けされた構成クラスを使用する
- 検証属性（Required、NotEmptyOrWhitespace）を実装する
- 設定に IConfiguration バインディングを使用する
- appsettings.json 構成ファイルをサポートする

## Semantic Kernel と AI 統合

- AI 操作に Microsoft.SemanticKernel を使用する
- 適切なカーネル構成とサービス登録を実装する
- AI モデル設定（ChatCompletion、Embedding など）を処理する
- 信頼性の高い AI 応答のために構造化出力パターンを使用する

## エラーハンドリングとログ

- Microsoft.Extensions.Logging で構造化ログを使用する
- 意味のあるコンテキストでスコープログを含める
- 説明的なメッセージで特定の例外をスローする
- 予期される失敗シナリオに try-catch ブロックを使用する

## パフォーマンスとセキュリティ

- 該当する場合は C# 12+ 機能と .NET 8 最適化を使用する
- 適切な入力検証とサニタイゼーションを実装する
- データベース操作にパラメーター化クエリを使用する
- AI/ML 操作でセキュアコーディングプラクティスに従う

## コード品質

- SOLID 原則のコンプライアンスを確保する
- 基底クラスとユーティリティによるコード重複を避ける
- ドメイン概念を反映する意味のある名前を使用する
- メソッドを焦点を絞って凝集性を保つ
- リソースの適切な廃棄パターンを実装する