---
applyTo: '*'
description: 'QuarkusとHTTP SSE転送を使用したMCPサーバーの開発標準と指針'
---
# Quarkus MCPサーバー

Java 21、Quarkus、HTTP SSE転送を使用してMCPサーバーを構築する。

## スタック

- Quarkus FrameworkでのJava 21
- MCPサーバー拡張：`mcp-server-sse`
- 依存性注入のためのCDI
- MCPエンドポイント：`http://localhost:8080/mcp/sse`

## クイックスタート

```bash
quarkus create app --no-code -x rest-client-jackson,qute,mcp-server-sse your-domain-mcp-server
```

## 構造

- 標準的なJava命名規則を使用する（PascalCaseクラス、camelCaseメソッド）
- パッケージで整理：`model`、`repository`、`service`、`mcp`
- 不変データモデルにはRecordタイプを使用する
- 不変データの状態管理はリポジトリレイヤーで管理する必要がある
- パブリックメソッドにJavadocを追加する

## MCPツール

- `@ApplicationScoped` CDI Beanのパブリックメソッドである必要がある
- `@Tool(name="tool_name", description="明確な説明")`を使用する
- `null`を返してはならない - 代わりにエラーメッセージを返す
- 常にパラメータを検証し、エラーを適切に処理する

## アーキテクチャ

- 関心の分離：MCPツール → サービス層 → リポジトリ
- 依存性注入には`@Inject`を使用する
- データ操作をスレッドセーフにする
- ヌルポインタ例外を避けるため`Optional<T>`を使用する

## 一般的な問題

- MCPツールにビジネスロジックを置かない（サービス層を使用する）
- ツールから例外を投げない（エラー文字列を返す）
- 入力パラメータの検証を忘れない
- エッジケース（null、空の入力）でテストする
