---
description: 'C# アプリケーション開発のためのコード作成規則 by @jgkim999'
applyTo: '**/*.cs'
---

# C# コード作成規則

## 命名規則 (Naming Conventions)

一貫した命名規則はコードの可読性の核心です。Microsoftのガイドラインに従うことを推奨します。

| 要素 | 命名規則 | 例 |
|------|-----------|------|
| インターフェース | 接頭辞 'I' + PascalCase | `IAsyncRepository`, `ILogger` |
| パブリックメンバー | パスカルケース (PascalCase) | `public int MaxCount;`, `public void GetData()` |
| パラメーター、ローカル変数 | キャメルケース (camelCase) | `int userCount`, `string customerName` |
| プライベート/内部フィールド | アンダースコア(_) + キャメルケース | `private string _connectionString;` |
| 定数 (const) | パスカルケース (PascalCase) | `public const int DefaultTimeout = 5000;` |
| ジェネリック型パラメーター | 接頭辞 'T' + 説明的な名前 | `TKey`, `TValue`, `TResult` |
| 非同期メソッド | 'Async' 接尾辞 | `GetUserAsync`, `DownloadFileAsync` |

## コードフォーマット及び可読性 (Formatting & Readability)

一貫したフォーマットはコードを視覚的に解析しやすくします。

| 項目 | 規則 | 説明 |
|------|------|------|
| インデント | 4つのスペースを使用 | タブの代わりに4つのスペースを使用します。csファイルは必ず4つのスペースを使用します。 |
| 括弧 | 常に中括弧 {} を使用 | 制御文(if, for, while等)が1行でも常に中括弧を使用します。 |
| 空行 | 論理的分離 | メソッド定義、プロパティ定義、論理的に分離されたコードブロック間に空行を追加します。 |
| 文の作成 | 1行に1つの文 | 1行には1つの文のみを記述します。 |
| var キーワード | 型が明確な場合のみ使用 | 変数の型を右側から明確に推論できる場合のみvarを使用します。 |
| 名前空間 | ファイルスコープ名前空間の使用 | C# 10以降では、ファイルスコープ名前空間を使用して不要なインデントを削減します。 |
| コメント | XML形式のコメント作成 | 作成したclassや関数には常にxml形式のコメントを記述します。 |

## 言語機能の使用 (Language Features)

最新のC#機能を活用してコードをより簡潔で効率的にしましょう。

| 機能 | 説明 | 例/参考 |
|------|------|------|
| 非同期プログラミング | I/Oバウンドタスクにasync/awaitを使用 | `async Task<string> GetDataAsync()` |
| ConfigureAwait | ライブラリコードでコンテキスト切り替えオーバーヘッドを削減 | `await SomeMethodAsync().ConfigureAwait(false)` |
| LINQ | コレクションデータのクエリと操作 | `users.Where(u => u.IsActive).ToList()` |
| 式ベースメンバー | 簡単なメソッド/プロパティを簡潔に表現 | `public string Name => _name;` |
| Nullable Reference Types | コンパイル時NullReferenceExceptionを防止 | `#nullable enable` |
| using 宣言 | IDisposableオブジェクトの簡潔な処理 | `using var stream = new FileStream(...);` |

## パフォーマンス及び例外処理 (Performance & Exception Handling)

堅牢で高速なアプリケーションのためのガイドラインです。

### 例外処理

処理できる具体的な例外のみをcatchしてください。catch (Exception)のような一般的な例外をキャッチすることは避けるべきです。

例外はプログラムフロー制御のために使用しないでください。例外は予期しないエラー状況にのみ使用されるべきです。

### パフォーマンス

文字列を反復的に連結する際は、+ 演算子の代わりにStringBuilderを使用してください。

Entity Framework Core使用時、読み取り専用クエリには .AsNoTracking() を使用してパフォーマンスを向上させてください。

不要なオブジェクト割り当てを避け、特にループ内では注意してください。

## セキュリティ (Security)

安全なコードを作成するための基本原則です。

| セキュリティ領域 | 規則 | 説明 |
|------|------|------|
| 入力値検証 | 全ての外部データを検証 | 外部（ユーザー、API等）から入力される全てのデータは信頼せず、常に妥当性を検査してください。 |
| SQLインジェクション防止 | パラメーター化クエリの使用 | 常にパラメーター化クエリやEntity FrameworkのようなORMを使用してSQLインジェクション攻撃を防止してください。 |
| 機密データ保護 | 設定管理ツールの使用 | パスワード、接続文字列、APIキー等はソースコードにハードコードせず、Secret Manager、Azure Key Vault等を使用してください。 |

これらの規則をプロジェクトの .editorconfig ファイルとチームのコードレビュープロセスに統合して、継続的に高品質なコードを維持することを目標とするべきです。
