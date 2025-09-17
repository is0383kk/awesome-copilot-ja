---
applyTo: '*'
description: 'Quarkus開発標準と指示事項'
---

- Java 17以降を使用した高品質Quarkusアプリケーションのための指示事項。

## プロジェクト コンテキスト

- 最新Quarkusバージョン: 3.x
- Javaバージョン: 17以降
- ビルド管理にはMavenまたはGradleを使用。
- クリーンアーキテクチャ、保守性、パフォーマンスに重点を置く。

## 開発標準

  - 各クラス、メソッド、複雑なロジックに対して明確で簡潔なコメントを記述する。
  - 消費者の明確性を確保するため、パブリックAPIとメソッドにJavadocを使用する。
  - Javaの規約に従い、プロジェクト全体で一貫したコーディングスタイルを維持する。
  - 最適なパフォーマンスと保守性のため、Quarkusコーディング標準とベストプラクティスに準拠する。
  - パッケージ構成の明確性を確保し、Jakarta EEとMicroProfileの規約に従う。
  - recordsやsealed classesなど、適切な場合はJava 17以降の機能を使用する。


## 命名規則
  - クラス名にはPascalCaseを使用する（例：`ProductService`、`ProductResource`）。
  - メソッドと変数名にはcamelCaseを使用する（例：`findProductById`、`isProductAvailable`）。
  - 定数にはALL_CAPSを使用する（例：`DEFAULT_PAGE_SIZE`）。

## Quarkus
  - より高速な開発サイクルのため、Quarkus開発モードを活用する。
  - Quarkus拡張機能とベストプラクティスを使用してビルド時最適化を実装する。
  - 最適なパフォーマンスのため、GraalVMでネイティブビルドを設定する（例：quarkus-maven-pluginを使用）。
  - 一貫したログプラクティスのため、quarkusログ機能（JBoss、SL4JまたはJUL）を使用する。

### Quarkus固有のパターン
- `@Singleton`の代わりに`@ApplicationScoped`をシングルトンBeanに使用
- 依存性注入には`@Inject`を使用
- 従来のJPAリポジトリよりもPancheリポジトリを優先
- データを変更するサービスメソッドには`@Transactional`を使用
- 説明的なRESTエンドポイントパスで`@Path`を適用
- RESTリソースには`@Consumes(MediaType.APPLICATION_JSON)`と`@Produces(MediaType.APPLICATION_JSON)`を使用

### RESTリソース
- 常にJAX-RSアノテーション（`@Path`、`@GET`、`@POST`など）を使用
- 適切なHTTPステータスコード（200、201、400、404、500）を返す
- 複雑なレスポンスには`Response`クラスを使用
- try-catchブロックで適切なエラーハンドリングを含める
- Bean Validationアノテーションを使用して入力パラメーターを検証
- パブリックエンドポイントのレート制限を実装

### データアクセス
- 従来のJPAよりもPancheエンティティ（`PanacheEntity`を拡張）を優先
- 複雑なクエリにはPancheリポジトリ（`PanacheRepository<T>`）を使用
- データ変更には常に`@Transactional`を使用
- 複雑なデータベース操作には名前付きクエリを使用
- リストエンドポイントに適切なページネーションを実装


### 設定
- 単純な設定には`application.properties`または`application.yaml`を使用
- タイプセーフな設定クラスには`@ConfigProperty`を使用
- 機密データには環境変数を優先
- 異なる環境（dev、test、prod）にはプロファイルを使用


### テスト
- 統合テストには`@QuarkusTest`を使用
- 単体テストにはJUnit 5を使用
- ネイティブビルドテストには`@QuarkusIntegrationTest`を使用
- `@QuarkusTestResource`を使用して外部依存関係をモック
- RESTエンドポイントテストにはRestAssuredを使用（`@QuarkusTestResource`）
- データベースを変更するテストには`@Transactional`を使用
- データベース統合テストにはtest-containersを使用

### 使用してはいけないパターン:
- テストでフィールドインジェクションを使用しない（コンストラクタインジェクションを使用）
- 設定値をハードコードしない
- 例外を無視しない


## 開発ワークフロー

### 新機能を作成する際：
1. 適切な検証を持つエンティティを作成
2. カスタムクエリでリポジトリを作成
3. ビジネスロジックでサービスを作成
4. 適切なエンドポイントでRESTリソースを作成
5. 包括的なテストを作成
6. 適切なエラーハンドリングを追加
7. ドキュメントを更新

## セキュリティ考慮事項

### セキュリティを実装する際：
- Quarkus Securityエクステンション（例：`quarkus-smallrye-jwt`、`quarkus-oidc`）を使用。
- MicroProfile JWTまたはOIDCを使用してロールベースアクセス制御（RBAC）を実装。
- すべての入力パラメーターを検証
