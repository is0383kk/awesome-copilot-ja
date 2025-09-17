---
applyTo: "**/*.Tests.ps1"
description: "Pester v5規約に基づくPowerShell Pesterテストのベストプラクティス"
---

# PowerShell Pester v5 テストガイドライン

このガイドは、PowerShell Pester v5 モジュールを使用して自動化されたテストを作成するための PowerShell 固有の指示を提供します。一般的な PowerShell スクリプト作成のベストプラクティスについては、[powershell.instructions.md](./powershell.instructions.md)の PowerShell コマンドレット開発ガイドラインに従ってください。

## ファイル命名と構造

- **ファイル規約:** `*.Tests.ps1`命名パターンを使用
- **配置:** テスト対象のコードの隣または専用テストディレクトリにテストファイルを配置
- **インポートパターン:** `BeforeAll { . $PSScriptRoot/FunctionName.ps1 }`を使用してテスト対象の関数をインポート
- **直接コードなし:** すべてのコードを Pester ブロック（`BeforeAll`、`Describe`、`Context`、`It`など）内に配置

## テスト構造の階層

```powershell
BeforeAll { # テスト対象の関数をインポート }
Describe 'FunctionName' {
    Context 'When condition' {
        BeforeAll { # コンテキストのセットアップ }
        It 'Should behavior' { # 個別のテスト }
        AfterAll { # コンテキストのクリーンアップ }
    }
}
```

## コアキーワード

- **`Describe`**: トップレベルのグループ化、通常はテスト対象の関数名
- **`Context`**: 特定のシナリオのための Describe 内のサブグループ
- **`It`**: 個別のテストケース、説明的な名前を使用
- **`Should`**: テスト検証のためのアサーションキーワード
- **`BeforeAll/AfterAll`**: ブロックごとに一度だけセットアップ/ティアダウン
- **`BeforeEach/AfterEach`**: 各テストの前後でセットアップ/ティアダウン

## セットアップとティアダウン

- **`BeforeAll`**: 含まれるブロックの開始時に一度実行、高コストな操作に使用
- **`BeforeEach`**: ブロック内のすべての`It`の前に実行、テスト固有のセットアップに使用
- **`AfterEach`**: すべての`It`の後に実行、テストが失敗しても保証される
- **`AfterAll`**: ブロックの終了時に一度実行、クリーンアップに使用
- **変数スコープ**: `BeforeAll`変数は子ブロックで利用可能（読み取り専用）、`BeforeEach/It/AfterEach`は同じスコープを共有

## アサーション（Should）

- **基本比較**: `-Be`, `-BeExactly`, `-Not -Be`
- **コレクション**: `-Contain`, `-BeIn`, `-HaveCount`
- **数値**: `-BeGreaterThan`, `-BeLessThan`, `-BeGreaterOrEqual`
- **文字列**: `-Match`, `-Like`, `-BeNullOrEmpty`
- **型**: `-BeOfType`, `-BeTrue`, `-BeFalse`
- **ファイル**: `-Exist`, `-FileContentMatch`
- **例外**: `-Throw`, `-Not -Throw`

## モッキング

- **`Mock CommandName { ScriptBlock }`**: コマンドの動作を置き換える
- **`-ParameterFilter`**: パラメータが条件に一致する場合のみモック
- **`-Verifiable`**: 検証が必要なモックとしてマーク
- **`Should -Invoke`**: モックが特定の回数呼び出されたことを検証
- **`Should -InvokeVerifiable`**: すべての検証可能なモックが呼び出されたことを検証
- **スコープ**: モックはデフォルトで含まれるブロックスコープ

```powershell
Mock Get-Service { @{ Status = 'Running' } } -ParameterFilter { $Name -eq 'TestService' }
Should -Invoke Get-Service -Exactly 1 -ParameterFilter { $Name -eq 'TestService' }
```

## テストケース（データ駆動テスト）

パラメータ化されたテストには`-TestCases`または`-ForEach`を使用：

```powershell
It 'Should return <Expected> for <Input>' -TestCases @(
    @{ Input = 'value1'; Expected = 'result1' }
    @{ Input = 'value2'; Expected = 'result2' }
) {
    Get-Function $Input | Should -Be $Expected
}
```

## データ駆動テスト

- **`-ForEach`**: データから複数のテストを生成するため`Describe`、`Context`、`It`で利用可能
- **`-TestCases`**: `It`ブロックでの`-ForEach`の別名（後方互換性）
- **ハッシュテーブルデータ**: 各項目はテストで利用可能な変数を定義（例：`@{ Name = 'value'; Expected = 'result' }`）
- **配列データ**: 現在の項目に`$_`変数を使用
- **テンプレート**: 動的展開のためにテスト名で`<変数名>`を使用

```powershell
# ハッシュテーブルアプローチ
It 'Returns <Expected> for <Name>' -ForEach @(
    @{ Name = 'test1'; Expected = 'result1' }
    @{ Name = 'test2'; Expected = 'result2' }
) { Get-Function $Name | Should -Be $Expected }

# 配列アプローチ
It 'Contains <_>' -ForEach 'item1', 'item2' { Get-Collection | Should -Contain $_ }
```

## タグ

- **利用可能**: `Describe`、`Context`、`It`ブロック
- **フィルタリング**: `Invoke-Pester`で`-TagFilter`と`-ExcludeTagFilter`を使用
- **ワイルドカード**: タグは柔軟なフィルタリングのため`-like`ワイルドカードをサポート

```powershell
Describe 'Function' -Tag 'Unit' {
    It 'Should work' -Tag 'Fast', 'Stable' { }
    It 'Should be slow' -Tag 'Slow', 'Integration' { }
}

# 高速なユニットテストのみ実行
Invoke-Pester -TagFilter 'Unit' -ExcludeTagFilter 'Slow'
```

## スキップ

- **`-Skip`**: テストをスキップするために`Describe`、`Context`、`It`で利用可能
- **条件付き**: 動的スキップには`-Skip:$condition`を使用
- **ランタイムスキップ**: テスト実行中に`Set-ItResult -Skipped`を使用（セットアップ/ティアダウンは実行される）

```powershell
It 'Should work on Windows' -Skip:(-not $IsWindows) { }
Context 'Integration tests' -Skip { }
```

## エラーハンドリング

- **失敗時継続**: 複数の失敗を収集するために`Should.ErrorAction = 'Continue'`を使用
- **重要な箇所で停止**: 前提条件に`-ErrorAction Stop`を使用
- **例外のテスト**: 例外テストには`{ Code } | Should -Throw`を使用

## ベストプラクティス

- **説明的な名前**: 動作を説明する明確なテスト説明を使用
- **AAA パターン**: 準備（Arrange）、実行（Act）、検証（Assert）
- **分離されたテスト**: 各テストは独立している必要がある
- **別名を避ける**: 完全なコマンドレット名を使用（`?`ではなく`Where-Object`）
- **単一責任**: 可能な場合はテストあたり一つのアサーション
- **テストファイルの構成**: 関連するテストを Context ブロックでグループ化。Context ブロックはネスト可能

## テストパターンの例

```powershell
BeforeAll {
    . $PSScriptRoot/Get-UserInfo.ps1
}

Describe 'Get-UserInfo' {
    Context 'When user exists' {
        BeforeAll {
            Mock Get-ADUser { @{ Name = 'TestUser'; Enabled = $true } }
        }

        It 'Should return user object' {
            $result = Get-UserInfo -Username 'TestUser'
            $result | Should -Not -BeNullOrEmpty
            $result.Name | Should -Be 'TestUser'
        }

        It 'Should call Get-ADUser once' {
            Get-UserInfo -Username 'TestUser'
            Should -Invoke Get-ADUser -Exactly 1
        }
    }

    Context 'When user does not exist' {
        BeforeAll {
            Mock Get-ADUser { throw "User not found" }
        }

        It 'Should throw exception' {
            { Get-UserInfo -Username 'NonExistent' } | Should -Throw "*not found*"
        }
    }
}
```

## 構成

構成は実行動作を制御するために`Invoke-Pester`を呼び出す際にテストファイル**外で**定義されます。

```powershell
# 構成を作成（Pester 5.2以降）
$config = New-PesterConfiguration
$config.Run.Path = './Tests'
$config.Output.Verbosity = 'Detailed'
$config.TestResult.Enabled = $true
$config.TestResult.OutputFormat = 'NUnitXml'
$config.Should.ErrorAction = 'Continue'
Invoke-Pester -Configuration $config
```

**主要セクション**: Run（Path、Exit）、Filter（Tag、ExcludeTag）、Output（Verbosity）、TestResult（Enabled、OutputFormat）、CodeCoverage（Enabled、Path）、Should（ErrorAction）、Debug
