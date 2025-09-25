---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems', 'search']
description: 'データ駆動テストを含む TUnit 単体テストのベストプラクティスを取得する'
---

# TUnit のベストプラクティス

標準的なテストアプローチとデータ駆動テストアプローチの両方をカバーし、TUnit を使って効果的な単体テストを書くことをサポートします。

## プロジェクトセットアップ

- 命名規則 `[ProjectName].Tests` で別のテストプロジェクトを使用する
- TUnit パッケージとフルーエント・アサーション用の TUnit.Assertions を参照する
- テスト対象のクラスに対応するテストクラスを作成する（例：`Calculator` に対して `CalculatorTests`）
- テストの実行には .NET SDK テストコマンド `dotnet test` を使用する
- TUnit には .NET 8.0 以上が必要です

## テスト構造

- テストクラス属性は不要（xUnit/NUnit のようなものではない）
- テストメソッドには `[Test]` 属性を使用する（xUnit のような `[Fact]` ではない）
- AAA（Arrange-Act-Assert）パターンに従う
- `MethodName_Scenario_ExpectedBehavior` パターンでテストに名前を付ける
- セットアップとクリーンアップにはライフサイクルフック `[Before(Test)]` と `[After(Test)]` を使用する
- クラス内のテスト間で共有コンテキストには `[Before(Class)]` と `[After(Class)]` を使用する
- テストクラス間で共有コンテキストには `[Before(Assembly)]` と `[After(Assembly)]` を使用する
- TUnit は `[Before(TestSession)]` や `[After(TestSession)]` などの高度なライフサイクルフックをサポートします

## 標準テスト

- テストは単一の動作に焦点を当てる
- 1つのテストメソッドで複数の動作をテストしない
- `await Assert.That()` で TUnit のフルーエントアサーション構文を使用する
- テストケースを検証するために必要なアサーションのみを含める
- テストを独立させ、冪等にする（任意の順序で実行可能）
- テスト間の依存関係を避ける（必要に応じて `[DependsOn]` 属性を使用）

## データ駆動テスト

- インラインテストデータには `[Arguments]` 属性を使用する（xUnit の `[InlineData]` に相当）
- メソッドベースのテストデータには `[MethodData]` を使用する（xUnit の `[MemberData]` に相当）
- クラスベースのテストデータには `[ClassData]` を使用する
- `ITestDataSource` を実装してカスタムデータソースを作成する
- データ駆動テストでは意味のあるパラメーター名を使用する
- 同じテストメソッドに複数の `[Arguments]` 属性を適用できます

## アサーション

- 値の等価性には `await Assert.That(value).IsEqualTo(expected)` を使用する
- 参照の等価性には `await Assert.That(value).IsSameReferenceAs(expected)` を使用する
- 論理値の条件には `await Assert.That(value).IsTrue()` や `await Assert.That(value).IsFalse()` を使用する
- コレクションには `await Assert.That(collection).Contains(item)` や `await Assert.That(collection).DoesNotContain(item)` を使用する
- 正規表現パターンマッチングには `await Assert.That(value).Matches(pattern)` を使用する
- 例外テストには `await Assert.That(action).Throws<TException>()` や `await Assert.That(asyncAction).ThrowsAsync<TException>()` を使用する
- `.And` 演算子でアサーションを連鎖する：`await Assert.That(value).IsNotNull().And.IsEqualTo(expected)`
- 代替条件には `.Or` 演算子を使用する：`await Assert.That(value).IsEqualTo(1).Or.IsEqualTo(2)`
- DateTime と数値比較の許容差には `.Within(tolerance)` を使用する
- すべてのアサーションは非同期であり、await する必要があります

## 高度な機能

- テストを複数回繰り返すには `[Repeat(n)]` を使用する
- 失敗時の自動再試行には `[Retry(n)]` を使用する
- 並列実行制限を制御するには `[ParallelLimit<T>]` を使用する
- 条件付きでテストをスキップするには `[Skip("reason")]` を使用する
- テストの依存関係を作成するには `[DependsOn(nameof(OtherTest))]` を使用する
- テストタイムアウトを設定するには `[Timeout(milliseconds)]` を使用する
- TUnit の基底属性を拡張してカスタム属性を作成する

## テストの組織

- 機能やコンポーネントごとにテストをグループ化する
- テスト分類には `[Category("CategoryName")]` を使用する
- カスタムテスト名には `[DisplayName("Custom Test Name")]` を使用する
- テスト診断と情報には `TestContext` の使用を検討する
- プラットフォーム固有のテストにはカスタム `[WindowsOnly]` のような条件属性を使用する

## パフォーマンスと並列実行

- TUnit はデフォルトでテストを並列実行します（明示的な設定が必要な xUnit とは異なる）
- 特定のテストで並列実行を無効にするには `[NotInParallel]` を使用する
- 同時実行を制御するには、カスタム制限クラスで `[ParallelLimit<T>]` を使用する
- 同じクラス内のテストはデフォルトで順次実行されます
- 負荷テストシナリオには `[Repeat(n)]` と `[ParallelLimit<T>]` を使用する

## xUnit からの移行

- `[Fact]` を `[Test]` に置き換える
- `[Theory]` を `[Test]` に置き換え、データには `[Arguments]` を使用する
- `[InlineData]` を `[Arguments]` に置き換える
- `[MemberData]` を `[MethodData]` に置き換える
- `Assert.Equal` を `await Assert.That(actual).IsEqualTo(expected)` に置き換える
- `Assert.True` を `await Assert.That(condition).IsTrue()` に置き換える
- `Assert.Throws<T>` を `await Assert.That(action).Throws<T>()` に置き換える
- コンストラクター/IDisposable を `[Before(Test)]`/`[After(Test)]` に置き換える
- `IClassFixture<T>` を `[Before(Class)]`/`[After(Class)]` に置き換える

**なぜ xUnit より TUnit なのか？**

TUnit は、非同期アサーション、より洗練されたライフサイクルフック、改善されたデータ駆動テスト機能など、xUnit にない高度な機能を備えた、モダンで高速かつ柔軟なテスト体験を提供します。TUnit のフルーエント・アサーションは、より明確で表現力豊かなテスト検証を提供し、複雑な .NET プロジェクトに特に適しています。