---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'データ駆動テストを含む MSTest 単体テストのベストプラクティスを取得する'
---

# MSTest のベストプラクティス

標準的なテストアプローチとデータ駆動テストアプローチの両方をカバーし、MSTest を使って効果的な単体テストを書くことをサポートします。

## プロジェクトセットアップ

- 命名規則 `[ProjectName].Tests` で別のテストプロジェクトを使用する
- MSTest パッケージを参照する
- テスト対象のクラスに対応するテストクラスを作成する（例：`Calculator` に対して `CalculatorTests`）
- テストの実行には .NET SDK テストコマンド `dotnet test` を使用する

## テスト構造

- テストクラスには `[TestClass]` 属性を使用する
- テストメソッドには `[TestMethod]` 属性を使用する
- AAA（Arrange-Act-Assert）パターンに従う
- `MethodName_Scenario_ExpectedBehavior` パターンでテストに名前を付ける
- テストごとのセットアップとクリーンアップには `[TestInitialize]` と `[TestCleanup]` を使用する
- クラスごとのセットアップとクリーンアップには `[ClassInitialize]` と `[ClassCleanup]` を使用する
- アセンブリレベルのセットアップとクリーンアップには `[AssemblyInitialize]` と `[AssemblyCleanup]` を使用する

## 標準テスト

- テストは単一の動作に焦点を当てる
- 1つのテストメソッドで複数の動作をテストしない
- 意図を表現する明確なアサーションを使用する
- テストケースを検証するために必要なアサーションのみを含める
- テストを独立させ、冪等にする（任意の順序で実行可能）
- テスト間の依存関係を避ける

## データ駆動テスト

- `[TestMethod]` とデータソース属性を組み合わせて使用する
- インラインテストデータには `[DataRow]` を使用する
- プログラムで生成されたテストデータには `[DynamicData]` を使用する
- テストにメタデータを追加するには `[TestProperty]` を使用する
- データ駆動テストでは意味のあるパラメーター名を使用する

## アサーション

- 値の等価性には `Assert.AreEqual` を使用する
- 参照の等価性には `Assert.AreSame` を使用する
- 論理値の条件には `Assert.IsTrue`/`Assert.IsFalse` を使用する
- コレクションの比較には `CollectionAssert` を使用する
- 文字列固有のアサーションには `StringAssert` を使用する
- 例外をテストするには `Assert.Throws<T>` を使用する
- アサーションは本質的にシンプルにし、失敗時の明確性のためにメッセージを提供する

## モッキングと分離

- MSTest と併用して Moq や NSubstitute の使用を検討する
- テスト対象のユニットを分離するために依存関係をモックする
- モッキングを容易にするためにインターフェースを使用する
- 複雑なテストセットアップには DI コンテナーの使用を検討する

## テストの組織

- 機能やコンポーネントごとにテストをグループ化する
- `[TestCategory("Category")]` でテストカテゴリを使用する
- 重要なテストには `[Priority(1)]` でテスト優先度を使用する
- 所有者を示すために `[Owner("DeveloperName")]` を使用する