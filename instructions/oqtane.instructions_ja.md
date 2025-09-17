---
description: 'Oqtaneモジュールパターン'
applyTo: '**/*.razor, **/*.razor.cs, **/*.razor.css'
---

## Blazorコードスタイルと構造

- イディオマティックで効率的なBlazorとC#コードを書く。
- .NETとBlazor規約に従う。
- コンポーネントベースUI開発にはRazorコンポーネントを適切に使用する。
- コンポーネントベースUI開発にはBlazorコンポーネントを適切に使用する。
- 小さなコンポーネントにはインライン関数を優先するが、複雑なロジックはコードビハインドまたはサービスクラスに分離する。
- Async/awaitは該当する箇所でノンブロッキングUI操作を確保するために使用すべき。

## 命名規則

- コンポーネント名、メソッド名、パブリックメンバーにはPascalCaseに従う。
- プライベートフィールドとローカル変数にはcamelCaseを使用する。
- インターフェース名には「I」を接頭辞にする（例：IUserService）。

## Blazorおよび.NET固有ガイドライン

- コンポーネントライフサイクルのBlazor組み込み機能を活用する（例：OnInitializedAsync、OnParametersSetAsync）。
- @bindでデータバインディングを効果的に使用する。
- BlazorでサービスのDependency Injectionを活用する。
- 関心の分離に従ってBlazorコンポーネントとサービスを構造化する。
- 常に最新版のC#を使用し、現在はrecord型、パターンマッチング、global usingsなどのC# 13機能を使用する。

## Oqtane固有ガイドライン
- [メインOqtaneリポジトリ](https://github.com/oqtane/oqtane.framework)のベースクラスとパターンを参照。
- モジュール開発にはクライアントサーバーパターンに従う。
- Clientプロジェクトにはmodulesフォルダーに様々なモジュールがある。
- クライアントモジュールの各アクションは、ModuleBaseから継承する個別のrazorファイルで、index.razorがデフォルトアクション。
- データ取得のような複雑なクライアント処理には、ServiceBaseから継承し、servicesフォルダーに存在するサービスクラスを作成する。各モジュールに1つのサービスクラス。
- クライアントサービスはServiceBaseメソッドを使用してサーバーエンドポイントを呼び出すべき。
- ServerプロジェクトにはMVCコントローラーが含まれ、クライアントサービス呼び出しに一致するモジュールごとに1つ。各コントローラーはDIで管理されるサーバーサイドサービスまたはリポジトリを呼び出す。
- Serverプロジェクトはモジュールにリポジトリパターンを使用し、コントローラーに一致するモジュールごとに1つのリポジトリクラス。

## エラーハンドリングと検証

- BlazorページとAPI呼び出しに適切なエラーハンドリングを実装する。
- ベースクラスからの組み込みOqtaneログ記録メソッドを使用する。
- バックエンドでエラー追跡にログ記録を使用し、ErrorBoundaryなどのツールでBlazorのUI レベルエラーキャプチャを検討する。
- フォームでFluentValidationまたはDataAnnotationsを使用して検証を実装する。

## Blazor APIとパフォーマンス最適化

- プロジェクト要件に基づいてBlazorサーバーサイドまたはWebAssemblyを最適に活用する。
- メインスレッドをブロックする可能性があるAPI呼び出しやUIアクションには非同期メソッド（async/await）を使用する。
- 不要なレンダリングを削減し、StateHasChanged()を効率的に使用してRazorコンポーネントを最適化する。
- 必要でない限り再レンダリングを避け、適切な場所でShouldRender()を使用してコンポーネントレンダーツリーを最小化する。
- ユーザーインタラクションを効率的に処理するためにEventCallbacksを使用し、イベントトリガー時に最小限のデータのみを渡す。

## キャッシュ戦略

- 特にBlazor Serverアプリで頻繁に使用されるデータにはインメモリキャッシュを実装する。軽量キャッシュソリューションにはIMemoryCacheを使用する。
- Blazor WebAssemblyでは、ユーザーセッション間でアプリケーション状態をキャッシュするためにlocalStorageまたはsessionStorageを活用する。
- 複数ユーザーまたはクライアント間で共有状態が必要な大規模アプリケーションには分散キャッシュ戦略（RedisまたはSQL Server Cacheなど）を検討する。
- データが変更される可能性が低い場合、冗長な呼び出しを避けるためにレスポンスを保存してAPI呼び出しをキャッシュし、ユーザーエクスペリエンスを向上させる。

## 状態管理ライブラリ

- コンポーネント間での基本的な状態共有にはBlazorの組み込みCascading ParametersとEventCallbacksを使用する。
- 適切な場合はPageStateやSiteStateなどのベースクラスに組み込まれたOqtane状態管理を使用する。
- アプリケーションが複雑になってもFluxorやBlazorStateのような追加依存関係の追加を避ける。
- Blazor WebAssemblyでのクライアントサイド状態永続化には、ページリロード間で状態を維持するためにBlazored.LocalStorageまたはBlazored.SessionStorageの使用を検討する。
- サーバーサイドBlazorでは、再レンダリングを最小化しながらユーザーセッション内で状態を管理するためにScoped ServicesとStateContainerパターンを使用する。

## API設計と統合

- 外部APIまたはサーバープロジェクトバックエンドと通信するためにサービスベースメソッドを使用する。
- try-catchを使用してAPI呼び出しのエラーハンドリングを実装し、UIで適切なユーザーフィードバックを提供する。

## Visual Studioでのテストとデバッグ

- すべての単体テストと統合テストはVisual Studio Enterpriseで行うべき。
- xUnit、NUnit、またはMSTestを使用してBlazorコンポーネントとサービスをテストする。
- テスト中の依存関係をモックするためにMoqまたはNSubstituteを使用する。
- ブラウザー開発者ツールとバックエンドおよびサーバーサイド問題にはVisual Studioのデバッグツールを使用してBlazor UI問題をデバッグする。
- パフォーマンスプロファイリングと最適化には、Visual Studioの診断ツールに依存する。

## セキュリティと認証

- User.RolesなどのOqtaneベースクラスメンバーを使用して認証と認可を実装する。
- すべてのWeb通信にHTTPSを使用し、適切なCORSポリシーが実装されていることを確認する。
