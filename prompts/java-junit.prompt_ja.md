---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'データ駆動テストを含む JUnit 5 単体テストのベストプラクティスを取得します'
---

# JUnit 5+ ベストプラクティス

JUnit 5 を使用した効果的な単体テストの作成を支援します。標準的なテストとデータ駆動テストアプローチの両方をカバーします。

## プロジェクトセットアップ

- 標準的な Maven または Gradle プロジェクト構造を使用します。
- テストソースコードは `src/test/java` に配置します。
- パラメーター化テストのために `junit-jupiter-api`、`junit-jupiter-engine`、`junit-jupiter-params` の依存関係を含めます。
- テストを実行するためにビルドツールコマンドを使用：`mvn test` または `gradle test`。

## テスト構造

- テストクラスは `Test` サフィックスを持つべきです（例：`Calculator` クラスの場合は `CalculatorTest`）。
- テストメソッドには `@Test` を使用します。
- Arrange-Act-Assert（AAA）パターンに従います。
- 説明的な規則を使ってテストに名前を付けます（例：`methodName_should_expectedBehavior_when_scenario`）。
- テストごとのセットアップとクリーンアップには `@BeforeEach` と `@AfterEach` を使用します。
- クラスごとのセットアップとクリーンアップには `@BeforeAll` と `@AfterAll` を使用します（static メソッドである必要があります）。
- テストクラスとメソッドに人間が読める名前を提供するために `@DisplayName` を使用します。

## 標準テスト

- テストは単一の動作に集中させます。
- 一つのテストメソッドで複数の条件をテストしないようにします。
- テストを独立的かつべき等（任意の順序で実行可能）にします。
- テスト間の依存関係を避けます。

## データ駆動（パラメーター化）テスト

- メソッドをパラメーター化テストとしてマークするために `@ParameterizedTest` を使用します。
- シンプルなリテラル値（文字列、整数など）には `@ValueSource` を使用します。
- テスト引数を `Stream`、`Collection` などとして提供するファクトリメソッドを参照するために `@MethodSource` を使用します。
- インラインのカンマ区切り値には `@CsvSource` を使用します。
- クラスパスから CSV ファイルを使用するために `@CsvFileSource` を使用します。
- enum 定数を使用するために `@EnumSource` を使用します。

## アサーション

- `org.junit.jupiter.api.Assertions` の static メソッドを使用します（例：`assertEquals`、`assertTrue`、`assertNotNull`）。
- より流暢で読みやすいアサーションには、AssertJ（`assertThat(...).is...`）などのライブラリの使用を検討します。
- 例外をテストするために `assertThrows` または `assertDoesNotThrow` を使用します。
- 関連するアサーションを `assertAll` でグループ化して、テストが失敗する前にすべてのアサーションがチェックされることを確保します。
- 失敗時の明確さを提供するためにアサーションで説明的なメッセージを使用します。

## モッキングと分離

- 依存関係のモックオブジェクトを作成するために Mockito などのモッキングフレームワークを使用します。
- モック作成と注入を簡素化するために Mockito の `@Mock` と `@InjectMocks` アノテーションを使用します。
- モッキングを容易にするためにインターフェースを使用します。

## テスト組織

- パッケージを使用して機能やコンポーネント別にテストをグループ化します。
- テストをカテゴライズするために `@Tag` を使用します（例：`@Tag("fast")`、`@Tag("integration")`）。
- 厳密に必要な場合は、テスト実行順序を制御するために `@TestMethodOrder(MethodOrderer.OrderAnnotation.class)` と `@Order` を使用します。
- 理由を提供してテストメソッドやクラスを一時的にスキップするために `@Disabled` を使用します。
- より良い組織と構造のためにネストされた内部クラスでテストをグループ化するために `@Nested` を使用します。