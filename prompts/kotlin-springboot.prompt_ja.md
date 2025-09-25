---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'Spring BootとKotlinを使用したアプリケーション開発のベストプラクティスを取得します'
---

# Spring Boot with Kotlin ベストプラクティス

あなたの目標は、Kotlinを使用した高品質で慣用的なSpring Bootアプリケーションを書くお手伝いをすることです。

## プロジェクトセットアップと構造

- **ビルドツール：** Kotlinプラグイン（`kotlin-maven-plugin`または`org.jetbrains.kotlin.jvm`）を使用してMaven（`pom.xml`）またはGradle（`build.gradle`）を使用する。
- **Kotlinプラグイン：** JPA用には、`kotlin-jpa`プラグインを有効にして、ボイラープレートなしでエンティティクラスを自動的に`open`にする。
- **スターター：** 通常通りSpring Bootスターター（例：`spring-boot-starter-web`、`spring-boot-starter-data-jpa`）を使用する。
- **パッケージ構造：** レイヤーごとではなく、機能/ドメインごとにコードを整理する（例：`com.example.app.order`、`com.example.app.user`）。

## 依存性注入とコンポーネント

- **プライマリコンストラクタ：** 必須の依存性注入には常にプライマリコンストラクタを使用する。これがKotlinで最も慣用的で簡潔なアプローチです。
- **不変性：** プライマリコンストラクタでの依存関係を`private val`として宣言する。不変性を促進するため、どこでも`var`より`val`を優先する。
- **コンポーネントステレオタイプ：** Javaと同様に`@Service`、`@Repository`、`@RestController`アノテーションを使用する。

## 設定

- **外部化設定：** 読みやすさと階層構造のため`application.yml`を使用する。
- **型安全プロパティ：** `data class`と`@ConfigurationProperties`を使用して不変で型安全な設定オブジェクトを作成する。
- **プロファイル：** 環境固有の設定を管理するためにSpring Profiles（`application-dev.yml`、`application-prod.yml`）を使用する。
- **シークレット管理：** シークレットをハードコードしない。環境変数またはHashiCorp VaultやAWS Secrets Managerなどの専用シークレット管理ツールを使用する。

## Webレイヤー（コントローラー）

- **RESTful API：** 明確で一貫したRESTfulエンドポイントを設計する。
- **DTOのデータクラス：** すべてのDTOにKotlinの`data class`を使用する。これにより`equals()`、`hashCode()`、`toString()`、`copy()`が無料で提供され、不変性が促進される。
- **バリデーション：** DTOデータクラスでJava Bean Validation（JSR 380）とアノテーション（`@Valid`、`@NotNull`、`@Size`）を使用する。
- **エラーハンドリング：** 一貫したエラーレスポンスのために`@ControllerAdvice`と`@ExceptionHandler`を使用してグローバル例外ハンドラーを実装する。

## サービスレイヤー

- **ビジネスロジック：** `@Service`クラス内にビジネスロジックをカプセル化する。
- **ステートレス：** サービスはステートレスであるべきです。
- **トランザクション管理：** サービスメソッドで`@Transactional`を使用する。Kotlinでは、これをクラスまたは関数レベルで適用できます。

## データレイヤー（リポジトリ）

- **JPAエンティティ：** エンティティをクラスとして定義する。それらは`open`でなければならないことを覚えておいてください。これを自動的に処理するために`kotlin-jpa`コンパイラプラグインの使用を強く推奨します。
- **Null安全性：** Kotlinのnull安全性（`?`）を活用して、型レベルでどのエンティティフィールドがオプションまたは必須かを明確に定義する。
- **Spring Data JPA：** `JpaRepository`または`CrudRepository`を拡張してSpring Data JPAリポジトリを使用する。
- **コルーチン：** リアクティブアプリケーションでは、データレイヤーでKotlin CoroutinesのSpring Bootサポートを活用する。

## ロギング

- **コンパニオンオブジェクトロガー：** ロガーを宣言する慣用的な方法は、コンパニオンオブジェクトです。
  ```kotlin
  companion object {
      private val logger = LoggerFactory.getLogger(MyClass::class.java)
  }
  ```
- **パラメータ化ロギング：** パフォーマンスと明確さのため、パラメータ化メッセージ（`logger.info("Processing user {}...", userId)`）を使用する。

## テスト

- **JUnit 5：** JUnit 5はデフォルトでKotlinとシームレスに連携します。
- **慣用的テストライブラリ：** より流暢で慣用的なテストのために、アサーション用の**Kotest**とモック用の**MockK**の使用を検討してください。これらはKotlin向けに設計されており、より表現力豊かな構文を提供します。
- **テストスライス：** アプリケーションの特定部分をテストするために、`@WebMvcTest`や`@DataJpaTest`などのテストスライスアノテーションを使用する。
- **Testcontainers：** 実際のデータベース、メッセージブローカーなどを使用した信頼性の高い統合テストにはTestcontainersを使用する。

## コルーチンと非同期プログラミング

- **`suspend`関数：** ノンブロッキング非同期コードには、コントローラーとサービスで`suspend`関数を使用する。Spring Bootはコルーチンに対して優れたサポートを提供します。
- **構造化並行性：** コルーチンのライフサイクルを管理するために`coroutineScope`または`supervisorScope`を使用する。