---
description: '.NETFrameworkプロジェクトでの作業ガイダンス。プロジェクト構造、C#言語バージョン、NuGet管理、ベストプラクティスを含む。'
applyTo: '**/*.csproj, **/*.cs'
---

# .NET Framework 開発

## ビルドとコンパイルの要件
- ソリューションやプロジェクトのビルドには`dotnet build`ではなく、常に`msbuild /t:rebuild`を使用する

## プロジェクトファイル管理

### 非SDKスタイルプロジェクト構造
.NET Frameworkプロジェクトは、最新のSDKスタイルプロジェクトとは大幅に異なるレガシープロジェクト形式を使用します：

- **明示的ファイル包含**: すべての新しいソースファイルは`<Compile>`要素を使用してプロジェクトファイル（`.csproj`）に明示的に追加する**必要がある**
  - .NET FrameworkプロジェクトはSDKスタイルプロジェクトのようにディレクトリ内のファイルを自動的に含まない
  - 例: `<Compile Include="Path\To\NewFile.cs" />`

- **暗黙的インポートなし**: SDKスタイルプロジェクトとは異なり、.NET Frameworkプロジェクトは共通の名前空間やアセンブリを自動的にインポートしない
 
- **ビルド構成**: Debug/Release構成用の明示的な`<PropertyGroup>`セクションを含む

- **出力パス**: 明示的な`<OutputPath>`と`<IntermediateOutputPath>`の定義

- **ターゲットフレームワーク**: `<TargetFramework>`ではなく`<TargetFrameworkVersion>`を使用
  - 例: `<TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>`

## NuGet パッケージ管理
- .NET FrameworkプロジェクトでのNuGetパッケージのインストールと更新は、複数のファイルに対する協調的な変更を必要とする複雑な作業です。そのため、このプロジェクトでは**NuGetパッケージのインストールや更新を試行しない**でください。
- 代わりに、NuGet参照の変更が必要な場合は、Visual Studio NuGetパッケージマネージャーまたはVisual Studioパッケージマネージャーコンソールを使用してNuGetパッケージをインストールまたは更新するようユーザーに依頼してください。
- NuGetパッケージを推奨する際は、.NET Frameworkまたは.NET Standard 2.0と互換性があることを確認してください（.NET Coreや.NET 5+のみではなく）。

## C# 言語バージョンは 7.3
- このプロジェクトはC# 7.3機能のみに制限されています。以下の使用を避けてください：

### C# 8.0+機能（サポートされていない）:
  - Using宣言（`using var stream = ...`）
  - Await using文（`await using var resource = ...`）
  - Switch式（`variable switch { ... }`）
  - Null合体代入（`??=`）
  - 範囲とインデックス演算子（`array[1..^1]`、`array[^1]`）
  - デフォルトインターフェースメソッド
  - 構造体の読み取り専用メンバー
  - 静的ローカル関数
  - Null許容参照型（`string?`、`#nullable enable`）

### C# 9.0+機能（サポートされていない）:
  - レコード（`public record Person(string Name)`）
  - init専用プロパティ（`{ get; init; }`）
  - トップレベルプログラム（Mainメソッドなしのプログラム）
  - パターンマッチング拡張
  - ターゲット型指定new式（`List<string> list = new()`）

### C# 10+機能（サポートされていない）:
  - グローバルusing文
  - ファイルスコープ名前空間
  - レコード構造体
  - 必須メンバー

### 代わりに使用（C# 7.3互換）:
  - 中括弧付きの従来のusing文
  - switch式ではなくswitch文
  - null合体代入ではなく明示的なnullチェック
  - 手動インデックスによる配列スライス
  - デフォルトインターフェースメソッドではなく抽象クラスまたはインターフェース

## 環境考慮事項（Windows環境）
- バックスラッシュ付きのWindowsスタイルパスを使用（例：`C:\path\to\file.cs`）
- ターミナル操作を提案する際はWindows適切なコマンドを使用
- ファイルシステム操作を扱う際はWindows固有の動作を考慮

## 一般的な.NET Frameworkの落とし穴とベストプラクティス

### Async/Awaitパターン
- **ConfigureAwait(false)**: デッドロックを避けるため、ライブラリコードでは常に`ConfigureAwait(false)`を使用：
  ```csharp
  var result = await SomeAsyncMethod().ConfigureAwait(false);
  ```
- **sync-over-asyncを避ける**: `.Result`や`.Wait()`、`.GetAwaiter().GetResult()`を使用しない。これらのsync-over-asyncパターンはデッドロックと性能低下を引き起こす可能性があります。非同期呼び出しには常に`await`を使用してください。

### DateTime処理
- **タイムスタンプにはDateTimeOffsetを使用**: 絶対時点には`DateTime`より`DateTimeOffset`を優先
- **DateTimeKindを指定**: `DateTime`を使用する際は、常に`DateTimeKind.Utc`または`DateTimeKind.Local`を指定
- **カルチャ対応フォーマット**: シリアル化/解析には`CultureInfo.InvariantCulture`を使用

### 文字列操作
- **連結にはStringBuilder**: 複数の文字列連結には`StringBuilder`を使用
- **StringComparison**: 文字列操作では常に`StringComparison`を指定：
  ```csharp
  string.Equals(other, StringComparison.OrdinalIgnoreCase)
  ```

### メモリ管理
- **Disposeパターン**: アンマネージリソースに対して`IDisposable`を適切に実装
- **Using文**: `IDisposable`オブジェクトは常にusing文でラップ
- **大オブジェクトヒープを避ける**: LOH割り当てを避けるためオブジェクトを85KB未満に保つ

### 構成
- **ConfigurationManagerを使用**: `ConfigurationManager.AppSettings`を通じてアプリ設定にアクセス
- **接続文字列**: `<appSettings>`ではなく`<connectionStrings>`セクションに格納
- **変換**: 環境固有設定にはweb.config/app.config変換を使用

### 例外処理
- **具体的な例外**: 汎用的な`Exception`ではなく具体的な例外型をキャッチ
- **例外を隠さない**: 例外を適切にログ記録または再スロー
- **破棄可能リソースにはusingを使用**: 例外発生時でも適切なクリーンアップを保証

### パフォーマンス考慮事項
- **ボクシングを避ける**: 値型とジェネリクスでのボクシング/アンボクシングに注意
- **文字列インターン**: 頻繁に使用される文字列に対して`string.Intern()`を慎重に使用
- **遅延初期化**: 高コストなオブジェクト作成には`Lazy<T>`を使用
- **ホットパスでのリフレクションを避ける**: 可能な場合は`MethodInfo`、`PropertyInfo`オブジェクトをキャッシュ
