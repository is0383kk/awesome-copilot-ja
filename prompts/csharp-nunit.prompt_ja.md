---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'データ駆動テストを含む NUnit 単体テストのベストプラクティスを取得する'
---

# NUnit のベストプラクティス

標準的なテストアプローチとデータ駆動テストアプローチの両方をカバーし、NUnit を使って効果的な単体テストを書くことをサポートします。

## プロジェクトセットアップ

- 命名規則 `[ProjectName].Tests` で別のテストプロジェクトを使用する
- Microsoft.NET.Test.Sdk、NUnit、NUnit3TestAdapter パッケージを参照する
- テスト対象のクラスに対応するテストクラスを作成する（例：`Calculator` に対して `CalculatorTests`）
- テストの実行には .NET SDK テストコマンド `dotnet test` を使用する

## テスト構造

- テストクラスに `[TestFixture]` 属性を適用する
- テストメソッドには `[Test]` 属性を使用する
- AAA（Arrange-Act-Assert）パターンに従う
- `MethodName_Scenario_ExpectedBehavior` パターンでテストに名前を付ける
- テストごとのセットアップとクリーンアップには `[SetUp]` と `[TearDown]` を使用する
- クラスごとのセットアップとクリーンアップには `[OneTimeSetUp]` と `[OneTimeTearDown]` を使用する
- アセンブリレベルのセットアップとクリーンアップには `[SetUpFixture]` を使用する

## 標準テスト

- テストは単一の動作に焦点を当てる
- 1つのテストメソッドで複数の動作をテストしない
- 意図を表現する明確なアサーションを使用する
- テストケースを検証するために必要なアサーションのみを含める
- テストを独立させ、冪等にする（任意の順序で実行可能）
- テスト間の依存関係を避ける

## データ駆動テスト

- インラインテストデータには `[TestCase]` を使用する
- プログラムで生成されたテストデータには `[TestCaseSource]` を使用する
- シンプルなパラメーターの組み合わせには `[Values]` を使用する
- プロパティやメソッドベースのデータソースには `[ValueSource]` を使用する
- ランダムな数値テスト値には `[Random]` を使用する
- 連続する数値テスト値には `[Range]` を使用する
- 複数のパラメーターを組み合わせるには `[Combinatorial]` や `[Pairwise]` を使用する

## アサーション

- 制約モデルで `Assert.That` を使用する（NUnit 推奨スタイル）
- `Is.EqualTo`、`Is.SameAs`、`Contains.Item` などの制約を使用する
- シンプルな値の等価性には `Assert.AreEqual` を使用する（クラシックスタイル）
- コレクションの比較には `CollectionAssert` を使用する
- 文字列固有のアサーションには `StringAssert` を使用する
- 例外をテストするには `Assert.Throws<T>` や `Assert.ThrowsAsync<T>` を使用する
- 失敗時の明確性のために、アサーションで説明的なメッセージを使用する

## モッキングと分離

- NUnit と併用して Moq や NSubstitute の使用を検討する
- テスト対象のユニットを分離するために依存関係をモックする
- モッキングを容易にするためにインターフェースを使用する
- 複雑なテストセットアップには DI コンテナーの使用を検討する

## テストの組織

- 機能やコンポーネントごとにテストをグループ化する
- `[Category("CategoryName")]` でカテゴリを使用する
- 必要に応じてテスト実行順序を制御するには `[Order]` を使用する
- 所有者を示すために `[Author("DeveloperName")]` を使用する
- 追加のテスト情報を提供するには `[Description]` を使用する
- 自動実行されるべきでないテストには `[Explicit]` を検討する
- テストを一時的にスキップするには `[Ignore("Reason")]` を使用する