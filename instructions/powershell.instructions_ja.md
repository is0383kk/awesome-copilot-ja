---
applyTo: '**/*.ps1,**/*.psm1'
description: 'Microsoftガイドラインに基づくPowerShellコマンドレットとスクリプトのベストプラクティス'
---  

# PowerShellコマンドレット開発ガイドライン

このガイドは、GitHub Copilotが慣用的で安全で保守可能なスクリプトを生成するのに役立つPowerShell固有の指示を提供します。MicrosoftのPowerShellコマンドレット開発ガイドラインと整合性を保っています。

## 命名規則

- **動詞-名詞形式:**
  - 承認されたPowerShell動詞を使用する（Get-Verb）
  - 単数形の名詞を使用する
  - 動詞と名詞の両方にPascalCaseを使用する
  - 特殊文字とスペースを避ける

- **パラメーター名:**
  - PascalCaseを使用する
  - 明確で説明的な名前を選択する
  - 常に複数でない限り単数形を使用する
  - PowerShell標準名に従う

- **変数名:**
  - パブリック変数にはPascalCaseを使用する
  - プライベート変数にはcamelCaseを使用する
  - 省略形を避ける
  - 意味のある名前を使用する

- **エイリアスの回避:**
  - 完全なコマンドレット名を使用する
  - スクリプトでエイリアスを使用しない（例：gciの代わりにGet-ChildItemを使用）
  - カスタムエイリアスを文書化する
  - 完全なパラメーター名を使用する

### 例

```powershell
function Get-UserProfile {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Username,

        [Parameter()]
        [ValidateSet('Basic', 'Detailed')]
        [string]$ProfileType = 'Basic'
    )

    process {
        # ロジックをここに記述
    }
}
```

## パラメーター設計

- **標準パラメーター:**
  - 共通パラメーター名を使用する（`Path`、`Name`、`Force`）
  - 組み込みコマンドレット規約に従う
  - 専門用語にはエイリアスを使用する
  - パラメーターの目的を文書化する

- **パラメーター名:**
  - 常に複数でない限り単数形を使用する
  - 明確で説明的な名前を選択する
  - PowerShell規約に従う
  - PascalCase形式を使用する

- **型選択:**
  - 一般的な.NET型を使用する
  - 適切な検証を実装する
  - 限定されたオプションにはValidateSetを検討する
  - 可能な場合はタブ補完を有効にする

- **スイッチパラメーター:**
  - ブール型フラグには[switch]を使用する
  - $true/$falseパラメーターを避ける
  - 省略時はデフォルトで$falseとする
  - 明確なアクション名を使用する

### 例

```powershell
function Set-ResourceConfiguration {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Name,
        
        [Parameter()]
        [ValidateSet('Dev', 'Test', 'Prod')]
        [string]$Environment = 'Dev',
        
        [Parameter()]
        [switch]$Force,
        
        [Parameter()]
        [ValidateNotNullOrEmpty()]
        [string[]]$Tags
    )
    
    process {
        # ロジックをここに記述
    }
}
```

## パイプラインと出力

- **パイプライン入力:**
  - 直接オブジェクト入力には`ValueFromPipeline`を使用する
  - プロパティマッピングには`ValueFromPipelineByPropertyName`を使用する
  - パイプライン処理にはBegin/Process/Endブロックを実装する
  - パイプライン入力要件を文書化する

- **出力オブジェクト:**
  - フォーマットされたテキストではなくリッチオブジェクトを返す
  - 構造化データにはPSCustomObjectを使用する
  - データ出力にWrite-Hostを避ける
  - 下流コマンドレット処理を有効にする

- **パイプラインストリーミング:**
  - 一度に一つのオブジェクトを出力する
  - ストリーミングにはprocessブロックを使用する
  - 大きな配列の収集を避ける
  - 即座の処理を有効にする

- **PassThruパターン:**
  - アクションコマンドレットではデフォルトで出力なしとする
  - オブジェクトリターンには`-PassThru`スイッチを実装する
  - `-PassThru`で変更/作成されたオブジェクトを返す
  - ステータス更新にはverbose/warningを使用する

### 例

```powershell
function Update-ResourceStatus {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory, ValueFromPipeline, ValueFromPipelineByPropertyName)]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateSet('Active', 'Inactive', 'Maintenance')]
        [string]$Status,

        [Parameter()]
        [switch]$PassThru
    )

    begin {
        Write-Verbose "リソースステータス更新プロセスを開始します"
        $timestamp = Get-Date
    }

    process {
        # 各リソースを個別に処理
        Write-Verbose "リソースを処理中: $Name"
        
        $resource = [PSCustomObject]@{
            Name = $Name
            Status = $Status
            LastUpdated = $timestamp
            UpdatedBy = $env:USERNAME
        }

        # PassThruが指定された場合のみ出力
        if ($PassThru) {
            Write-Output $resource
        }
    }

    end {
        Write-Verbose "リソースステータス更新プロセスが完了しました"
    }
}
```

## エラーハンドリングと安全性

- **ShouldProcess実装:**
  - `[CmdletBinding(SupportsShouldProcess = $true)]`を使用する
  - 適切な`ConfirmImpact`レベルを設定する
  - システム変更には`$PSCmdlet.ShouldProcess()`を呼び出す
  - 追加確認には`ShouldContinue()`を使用する

- **メッセージストリーム:**
  - `-Verbose`での操作詳細には`Write-Verbose`
  - 警告条件には`Write-Warning`
  - 非終了エラーには`Write-Error`
  - 終了エラーには`throw`
  - ユーザーインターフェーステキスト以外では`Write-Host`を避ける

- **エラーハンドリングパターン:**
  - エラー管理にはtry/catchブロックを使用する
  - 適切なErrorAction設定を設定する
  - 意味のあるエラーメッセージを返す
  - 必要に応じてErrorVariableを使用する
  - 適切な終了vs非終了エラーハンドリングを含める

- **非対話型設計:**
  - パラメーター経由で入力を受け入れる
  - スクリプトで`Read-Host`を避ける
  - 自動化シナリオをサポートする
  - すべての必要な入力を文書化する

### 例

```powershell
function Remove-UserAccount {
    [CmdletBinding(SupportsShouldProcess = $true, ConfirmImpact = 'High')]
    param(
        [Parameter(Mandatory, ValueFromPipeline)]
        [ValidateNotNullOrEmpty()]
        [string]$Username,

        [Parameter()]
        [switch]$Force
    )

    begin {
        Write-Verbose "ユーザーアカウント削除プロセスを開始します"
        $ErrorActionPreference = 'Stop'
    }

    process {
        try {
            # 検証
            if (-not (Test-UserExists -Username $Username)) {
                Write-Error "ユーザーアカウント '$Username' が見つかりません"
                return
            }

            # 確認
            $shouldProcessMessage = "ユーザーアカウント '$Username' を削除"
            if ($Force -or $PSCmdlet.ShouldProcess($Username, $shouldProcessMessage)) {
                Write-Verbose "ユーザーアカウントを削除中: $Username"
                
                # メイン操作
                Remove-ADUser -Identity $Username -ErrorAction Stop
                Write-Warning "ユーザーアカウント '$Username' が削除されました"
            }
        }
        catch [Microsoft.ActiveDirectory.Management.ADException] {
            Write-Error "Active Directoryエラー: $_"
            throw
        }
        catch {
            Write-Error "ユーザーアカウント削除の予期しないエラー: $_"
            throw
        }
    }

    end {
        Write-Verbose "ユーザーアカウント削除プロセスが完了しました"
    }
}
```

## ドキュメントとスタイル

- **コメントベースヘルプ:** パブリック向けの関数またはコマンドレットにはコメントベースヘルプを含める。関数内に`<# ... #>`ヘルプコメントを追加し、少なくとも以下を含める：
  - `.SYNOPSIS` 簡潔な説明
  - `.DESCRIPTION` 詳細な説明
  - `.EXAMPLE` 実用的な使用法のセクション
  - `.PARAMETER` 説明
  - `.OUTPUTS` 返される出力の型
  - `.NOTES` 追加情報

- **一貫したフォーマット:**
  - 一貫したPowerShellスタイルに従う
  - 適切なインデント（4スペース推奨）を使用する
  - 文と同じ行に開始中括弧を配置
  - 新しい行に終了中括弧を配置
  - パイプライン演算子の後に改行を使用
  - 関数とパラメーター名にPascalCaseを使用
  - 不要な空白を避ける

- **パイプラインサポート:**
  - パイプライン関数にはBegin/Process/Endブロックを実装する
  - 適切な場合はValueFromPipelineを使用する
  - プロパティ名によるパイプライン入力をサポートする
  - フォーマットされたテキストではなく適切なオブジェクトを返す

- **エイリアスを避ける:** 完全なコマンドレット名とパラメーターを使用する
  - スクリプトでエイリアスを使用しない（例：gciの代わりにGet-ChildItemを使用）；エイリアスは対話型シェル使用では許可される。
  - `?`や`where`の代わりに`Where-Object`を使用
  - `%`の代わりに`ForEach-Object`を使用
  - `ls`や`dir`の代わりに`Get-ChildItem`を使用

## 完全な例：エンドツーエンドコマンドレットパターン

```powershell
function New-Resource {
    [CmdletBinding(SupportsShouldProcess = $true, ConfirmImpact = 'Medium')]
    param(
        [Parameter(Mandatory = $true,
                   ValueFromPipeline = $true,
                   ValueFromPipelineByPropertyName = $true)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter()]
        [ValidateSet('Development', 'Production')]
        [string]$Environment = 'Development'
    )
    
    begin {
        Write-Verbose "リソース作成プロセスを開始します"
    }
    
    process {
        try {
            if ($PSCmdlet.ShouldProcess($Name, "新しいリソースを作成")) {
                # リソース作成ロジックをここに記述
                Write-Output ([PSCustomObject]@{
                    Name = $Name
                    Environment = $Environment
                    Created = Get-Date
                })
            }
        }
        catch {
            Write-Error "リソース作成に失敗しました: $_"
        }
    }
    
    end {
        Write-Verbose "リソース作成プロセスが完了しました"
    }
}
```
