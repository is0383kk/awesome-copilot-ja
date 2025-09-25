---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'データ駆動テストを含む XUnit 単体テストのベストプラクティスを取得する'
---

# XUnit のベストプラクティス

標準的なテストアプローチとデータ駆動テストアプローチの両方をカバーし、XUnit を使って効果的な単体テストを書くことをサポートします。

## プロジェクトセットアップ

- 命名規則 `[ProjectName].Tests` で別のテストプロジェクトを使用する
- Microsoft.NET.Test.Sdk、xunit、xunit.runner.visualstudio パッケージを参照する
- テスト対象のクラスに対応するテストクラスを作成する（例：`Calculator` に対して `CalculatorTests`）
- テストの実行には .NET SDK テストコマンド `dotnet test` を使用する

## テスト構造

- テストクラス属性は不要（MSTest/NUnit とは異なる）
- シンプルなテストには `[Fact]` 属性でファクトベーステストを使用する
- AAA（Arrange-Act-Assert）パターンに従う
- `MethodName_Scenario_ExpectedBehavior` パターンでテストに名前を付ける
- セットアップにはコンストラクターを使用し、クリーンアップには `IDisposable.Dispose()` を使用する
- クラス内のテスト間で共有コンテキストには `IClassFixture<T>` を使用する
- 複数のテストクラス間で共有コンテキストには `ICollectionFixture<T>` を使用する

## 標準テスト

- テストは単一の動作に焦点を当てる
- 1つのテストメソッドで複数の動作をテストしない
- 意図を表現する明確なアサーションを使用する
- テストケースを検証するために必要なアサーションのみを含める
- テストを独立させ、冪等にする（任意の順序で実行可能）
- テスト間の依存関係を避ける

## データ駆動テスト

- `[Theory]` とデータソース属性を組み合わせて使用する
- インラインテストデータには `[InlineData]` を使用する
- メソッドベースのテストデータには `[MemberData]` を使用する
- クラスベースのテストデータには `[ClassData]` を使用する
- `DataAttribute` を実装してカスタムデータ属性を作成する
- データ駆動テストでは意味のあるパラメーター名を使用する

## アサーション

- 値の等価性には `Assert.Equal` を使用する
- 参照の等価性には `Assert.Same` を使用する
- 論理値の条件には `Assert.True`/`Assert.False` を使用する
- コレクションには `Assert.Contains`/`Assert.DoesNotContain` を使用する
- 正規表現パターンマッチングには `Assert.Matches`/`Assert.DoesNotMatch` を使用する
- 例外をテストするには `Assert.Throws<T>` や `await Assert.ThrowsAsync<T>` を使用する
- より読みやすいアサーションのためにフルーエント・アサーションライブラリを使用する

## モッキングと分離

- XUnit と併用して Moq や NSubstitute の使用を検討する
- テスト対象のユニットを分離するために依存関係をモックする
- モッキングを容易にするためにインターフェースを使用する
- 複雑なテストセットアップには DI コンテナーの使用を検討する

## テストの組織

- 機能やコンポーネントごとにテストをグループ化する
- 分類には `[Trait("Category", "CategoryName")]` を使用する
- 共有依存関係を持つテストをグループ化するにはコレクションフィクスチャを使用する
- テスト診断には出力ヘルパー（`ITestOutputHelper`）の使用を検討する
- Fact/Theory 属性で `Skip = "reason"` を使ってテストを条件付きでスキップする