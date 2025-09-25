---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'Spring Boot でアプリケーションを開発するためのベストプラクティスを取得。'
---

# Spring Boot ベストプラクティス

確立されたベストプラクティスに従って高品質な Spring Boot アプリケーションを作成することをサポートします。

## プロジェクトセットアップ・構造

- **ビルドツール:** 依存関係管理に Maven（`pom.xml`）または Gradle（`build.gradle`）を使用。
- **スターター:** Spring Boot スターター（例：`spring-boot-starter-web`、`spring-boot-starter-data-jpa`）を使用して依存関係管理を簡素化。
- **パッケージ構造:** レイヤー別（例：`com.example.app.controller`、`com.example.app.service`）ではなく機能/ドメイン別（例：`com.example.app.order`、`com.example.app.user`）でコードを整理。

## 依存性注入・コンポーネント

- **コンストラクタインジェクション:** 必要な依存関係には常にコンストラクタベースのインジェクションを使用。これによりコンポーネントがテストしやすくなり、依存関係が明示的になる。
- **不変性:** 依存関係フィールドを `private final` として宣言。
- **コンポーネントステレオタイプ:** Bean を定義するために `@Component`、`@Service`、`@Repository`、`@Controller`/`@RestController` アノテーションを適切に使用。

## 設定

- **外部化設定:** 設定には `application.yml`（または `application.properties`）を使用。YAML は可読性と階層構造でしばしば好まれる。
- **型安全プロパティ:** `@ConfigurationProperties` を使用して設定を強く型付けされた Java オブジェクトにバインド。
- **プロファイル:** 環境固有の設定を管理するために Spring Profiles（`application-dev.yml`、`application-prod.yml`）を使用。
- **シークレット管理:** シークレットをハードコードしない。環境変数、または HashiCorp Vault や AWS Secrets Manager などの専用シークレット管理ツールを使用。

## Web 層（コントローラー）

- **RESTful API:** 明確で一貫性のある RESTful エンドポイントを設計。
- **DTO（データ転送オブジェクト）:** API 層でデータを公開・消費するために DTO を使用。JPA エンティティを直接クライアントに公開しない。
- **バリデーション:** リクエストペイロードを検証するために DTO で Java Bean Validation（JSR 380）とアノテーション（`@Valid`、`@NotNull`、`@Size`）を使用。
- **エラー処理:** 一貫性のあるエラーレスポンスを提供するために `@ControllerAdvice` と `@ExceptionHandler` を使用してグローバル例外ハンドラーを実装。

## サービス層

- **ビジネスロジック:** すべてのビジネスロジックを `@Service` クラス内にカプセル化。
- **ステートレス:** サービスはステートレスであるべき。
- **トランザクション管理:** データベーストランザクションを宣言的に管理するためにサービスメソッドで `@Transactional` を使用。必要な最も細かいレベルで適用。

## データ層（リポジトリ）

- **Spring Data JPA:** 標準的なデータベース操作のために `JpaRepository` または `CrudRepository` を拡張した Spring Data JPA リポジトリを使用。
- **カスタムクエリ:** 複雑なクエリには `@Query` または JPA Criteria API を使用。
- **プロジェクション:** データベースから必要なデータのみを取得するために DTO プロジェクションを使用。

## ログ

- **SLF4J:** ログには SLF4J API を使用。
- **ロガー宣言:** `private static final Logger logger = LoggerFactory.getLogger(MyClass.class);`
- **パラメータ化ログ:** パフォーマンスを向上させるために文字列連結の代わりにパラメータ化メッセージ（`logger.info("Processing user {}...", userId);`）を使用。

## テスト

- **ユニットテスト:** JUnit 5 と Mockito などのモッキングフレームワークを使用してサービスとコンポーネントのユニットテストを作成。
- **統合テスト:** Spring アプリケーションコンテキストをロードする統合テストには `@SpringBootTest` を使用。
- **テストスライス:** アプリケーションの特定部分を分離してテストするために `@WebMvcTest`（コントローラー用）や `@DataJpaTest`（リポジトリ用）などのテストスライスアノテーションを使用。
- **Testcontainers:** 実際のデータベース、メッセージブローカーなどとの信頼性の高い統合テストのために Testcontainers の使用を検討。

## セキュリティ

- **Spring Security:** 認証と認可に Spring Security を使用。
- **パスワードエンコーディング:** BCrypt などの強力なハッシュアルゴリズムを使用してパスワードを常にエンコード。
- **入力サニタイゼーション:** Spring Data JPA またはパラメータ化クエリを使用して SQL インジェクションを防止。出力を適切にエンコードしてクロスサイトスクリプティング（XSS）を防止。