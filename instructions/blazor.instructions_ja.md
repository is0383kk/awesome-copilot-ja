---
description: 'Blazorコンポーネントとアプリケーションパターン'
applyTo: '**/*.razor, **/*.razor.cs, **/*.razor.css'
---

# Blazor開発ガイドライン

## Blazorコードスタイルと構造

- 慣用的で効率的なBlazorとC#コードを書く
- .NETとBlazorの規則に従う
- コンポーネントベースのUI開発に適切にRazorコンポーネントを使用する
- 小さなコンポーネントにはインライン関数を推奨するが、複雑なロジックはコードビハインドまたはサービスクラスに分離する
- ノンブロッキングなUI操作を確保するため、該当する場所でAsync/awaitを使用する

## 命名規則

- コンポーネント名、メソッド名、公開メンバーにはPascalCaseに従う
- プライベートフィールドとローカル変数にはcamelCaseを使用する
- インターフェース名には「I」をプレフィックス付与（例：IUserService）

## Blazorと.NET固有のガイドライン

- コンポーネントライフサイクル用のBlazorの組み込み機能を活用する（例：OnInitializedAsync、OnParametersSetAsync）
- @bindでデータバインディングを効果的に使用する
- BlazorでサービスにDependency Injectionを活用する
- Separation of Concernsに従ってBlazorコンポーネントとサービスを構造化する
- 常に最新バージョンのC#を使用する（現在はC# 13のrecord型、パターンマッチング、グローバルusingなどの機能）

## エラーハンドリングと検証

- BlazorページとAPI呼び出し用の適切なエラーハンドリングを実装する
- バックエンドでのエラー追跡にログ記録を使用し、ErrorBoundaryなどのツールでBlazorのUIレベルのエラーをキャプチャすることを検討する
- フォームでFluentValidationまたはDataAnnotationsを使用して検証を実装する

## Blazor APIとパフォーマンス最適化

- プロジェクト要件に基づいてBlazorサーバーサイドまたはWebAssemblyを最適に活用する
- API呼び出しやメインスレッドをブロックする可能性のあるUIアクションには非同期メソッド（async/await）を使用する
- 不要なレンダリングを減らし、StateHasChanged()を効率的に使用してRazorコンポーネントを最適化する
- 必要でない限り再レンダリングを避け、適切な場合にShouldRender()を使用してコンポーネントレンダーツリーを最小化する
- ユーザーインタラクションを効率的に処理するためEventCallbacksを使用し、イベントをトリガーする際は最小限のデータのみを渡す

## キャッシュ戦略

- 特にBlazor Serverアプリでは、頻繁に使用されるデータにインメモリキャッシュを実装する。軽量なキャッシュソリューションにはIMemoryCacheを使用する
- Blazor WebAssemblyでは、ユーザーセッション間でアプリケーション状態をキャッシュするためlocalStorageまたはsessionStorageを活用する
- 複数のユーザーまたはクライアント間で共有状態が必要な大規模アプリケーションには、分散キャッシュ戦略（RedisやSQL Server Cacheなど）を検討する
- データが変更されにくい場合は、冗長な呼び出しを避けるため、レスポンスを保存してAPI呼び出しをキャッシュし、ユーザーエクスペリエンスを向上させる

## 状態管理ライブラリ

- コンポーネント間での基本的な状態共有にはBlazorの組み込みCascading ParametersとEventCallbacksを使用する
- アプリケーションが複雑になった場合は、FluxorやBlazorStateなどのライブラリを使用した高度な状態管理ソリューションを実装する
- Blazor WebAssemblyでのクライアント側状態永続化には、ページリロード間で状態を維持するためBlazored.LocalStorageまたはBlazored.SessionStorageの使用を検討する
- サーバーサイドBlazorでは、再レンダリングを最小化しながらユーザーセッション内で状態を管理するため、Scoped ServicesとStateContainerパターンを使用する

## API設計と統合

- 外部APIまたは独自のバックエンドとの通信にHttpClientまたは他の適切なサービスを使用する
- try-catchを使用してAPI呼び出し用のエラーハンドリングを実装し、UIで適切なユーザーフィードバックを提供する

## Visual Studioでのテストとデバッグ

- すべてのユニットテストと統合テストはVisual Studio Enterpriseで実行する
- xUnit、NUnit、またはMSTestを使用してBlazorコンポーネントとサービスをテストする
- テスト中の依存関係のモックにはMoqまたはNSubstituteを使用する
- Blazor UIの問題はブラウザ開発者ツールで、バックエンドとサーバーサイドの問題はVisual Studioのデバッグツールを使用してデバッグする
- パフォーマンスプロファイリングと最適化には、Visual Studioの診断ツールに依存する

## セキュリティと認証

- 必要に応じてBlazorアプリで認証と認可を実装する。API認証にはASP.NET IdentityまたはJWTトークンを使用する
- すべてのWeb通信にHTTPSを使用し、適切なCORSポリシーが実装されていることを確認する

## APIドキュメンテーションとSwagger

- バックエンドAPIサービスのAPIドキュメンテーションにSwagger/OpenAPIを使用する
- Swaggerドキュメンテーションの拡張のため、モデルとAPIメソッド用のXMLドキュメンテーションを確保する
