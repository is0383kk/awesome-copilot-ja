---
title: Power Platform コネクタスキーマ開発指示書
description: "JSON スキーマ定義を使用した Power Platform カスタムコネクタの包括的な開発ガイドライン。API定義（Swagger 2.0）、APIプロパティ、Microsoft拡張機能を含む設定構成をカバー。"
applyTo: "**/*.{json,md}"
---

# Power Platform コネクタスキーマ開発指示書

## プロジェクト概要

このワークスペースには、特に `paconn`（Power Apps コネクタ）ツール向けの Power Platform カスタムコネクタ用 JSON スキーマ定義が含まれています。スキーマは以下を検証し、IntelliSense を提供します：

- **API 定義**（Swagger 2.0 形式）
- **API プロパティ**（コネクタメタデータと構成）
- **設定**（環境とデプロイメント構成）

## ファイル構造の理解

### 1. apiDefinition.swagger.json

- **目的**: このファイルには、Power Platform 拡張機能を含む Swagger 2.0 API 定義が含まれています。
- **主な機能**:
  - info、paths、definitions などを含む標準 Swagger 2.0 プロパティ。
  - `x-ms-*`プレフィックスで始まる Microsoft 固有の拡張機能。
  - `date-no-tz`や`html`など Power Platform 専用に設計されたカスタム形式タイプ。
  - ランタイム柔軟性を提供する動的スキーマサポート。
  - OAuth2、API キー、基本認証をサポートするセキュリティ定義。

### 2. apiProperties.json

- **目的**: このファイルは、コネクタメタデータ、認証構成、ポリシー構成を定義します。
- **主要コンポーネント**:
  - **接続パラメータ**: OAuth、API キー、ゲートウェイ構成を含む様々な認証タイプをサポート。
  - **ポリシーテンプレートインスタンス**: コネクタのデータ変換とルーティングポリシーを処理。
  - **コネクタメタデータ**: パブリッシャー情報、機能、ブランディング要素を含む。

### 3. settings.json

- **目的**: このファイルは、paconn ツールの環境とデプロイメント構成設定を提供します。
- **構成オプション**:
  - 特定の Power Platform 環境を対象とする環境 GUID。
  - コネクタアセットと構成ファイルのファイルパスマッピング。
  - 本番環境とテスト環境（PROD/TIP1）の両方の API エンドポイント URL。
  - Power Platform サービスとの互換性を保証する API バージョン仕様。

## 開発ガイドライン

### API 定義（Swagger）を扱う場合

1. **常に Swagger 2.0 仕様に対して検証する** - スキーマは厳格な Swagger 2.0 準拠を強制します

2. **操作用 Microsoft 拡張機能**:

   - `x-ms-summary`: ユーザーフレンドリーな表示名を提供し、タイトルケース形式を使用してください。
   - `x-ms-visibility`: `important`、`advanced`、`internal`の値でパラメータの可視性を制御してください。
   - `x-ms-trigger`: `batch`または`single`の値で操作をトリガーとしてマークしてください。
   - `x-ms-trigger-hint`: トリガーを使用する際のユーザーガイドとなる有用なヒントテキストを提供してください。
   - `x-ms-trigger-metadata`: kind と mode プロパティを含むトリガー構成設定を定義してください。
   - `x-ms-notification`: リアルタイム通知のための Webhook 操作を構成してください。
   - `x-ms-pageable`: `nextLinkName`プロパティを指定してページング機能を有効にしてください。
   - `x-ms-safe-operation`: 副作用がない場合に POST 操作を安全としてマークしてください。
   - `x-ms-no-generic-test`: 特定の操作の自動テストを無効にしてください。
   - `x-ms-operation-context`: テスト目的の操作シミュレーション設定を構成してください。

3. **パラメータ用 Microsoft 拡張機能**:

   - `x-ms-dynamic-list`: API 呼び出しから取得される動的ドロップダウンリストを有効にしてください。
   - `x-ms-dynamic-values`: パラメータオプションを設定する動的値ソースを構成してください。
   - `x-ms-dynamic-tree`: ネストしたデータ構造の階層セレクターを作成してください。
   - `x-ms-dynamic-schema`: ユーザー選択に基づくランタイムスキーマ変更を許可してください。
   - `x-ms-dynamic-properties`: コンテキストに適応する動的プロパティ構成を使用してください。
   - `x-ms-enum-values`: より良いユーザーエクスペリエンスのため表示名を含む拡張 enum 定義を提供してください。
   - `x-ms-test-value`: テスト用のサンプル値を提供しますが、秘密や機密データは含めないでください。
   - `x-ms-trigger-value`: `value-collection`と`value-path`プロパティでトリガーパラメータ専用の値を指定してください。
   - `x-ms-url-encoding`: URL エンコーディングスタイルを`single`または`double`として指定してください（デフォルトは`single`）。
   - `x-ms-parameter-location`: API のパラメータ位置ヒントを提供してください（AutoRest 拡張 - Power Platform では無視されます）。
   - `x-ms-localizeDefaultValue`: デフォルトパラメータ値のローカライゼーションを有効にしてください。
   - `x-ms-skip-url-encoding`: パスパラメータの URL エンコーディングをスキップしてください（AutoRest 拡張 - Power Platform では無視されます）。

4. **スキーマ用 Microsoft 拡張機能**:

   - `x-ms-notification-url`: Webhook 構成の通知 URL としてスキーマプロパティをマークしてください。
   - `x-ms-media-kind`: `image`または`audio`の対応値でコンテンツのメディアタイプを指定してください。
   - `x-ms-enum`: 拡張 enum メタデータを提供してください（AutoRest 拡張 - Power Platform では無視されます）。
   - 上記のパラメータ拡張機能はすべて、スキーマプロパティにも適用され、スキーマ定義内で使用できることに注意してください。

5. **ルートレベル拡張機能**:

   - `x-ms-capabilities`: ファイルピッカーや testConnection 機能などのコネクタ機能を定義してください。
   - `x-ms-connector-metadata`: 標準プロパティを超える追加のコネクタメタデータを提供してください。
   - `x-ms-docs`: コネクタのドキュメント設定と参照を構成してください。
   - `x-ms-deployment-version`: デプロイメント管理のバージョン情報を追跡してください。
   - `x-ms-api-annotation`: 拡張機能のための API レベル注釈を追加してください。

6. **パスレベル拡張機能**:

   - `x-ms-notification-content`: Webhook パス項目の通知コンテンツスキーマを定義してください。

7. **操作レベル機能**:

   - `x-ms-capabilities`（操作レベル）: 大きなファイル転送のための`chunkTransfer`などの操作固有機能を有効にしてください。

8. **セキュリティの考慮事項**:

   - 適切な認証を確保するために API の適切な`securityDefinitions`を定義してください。
   - **複数のセキュリティ定義が許可されています** - 最大 2 つの認証方法を定義できます（例：oauth2 + apiKey、basic + apiKey）。
   - **例外**: "None"認証を使用する場合、同じコネクタ内に他のセキュリティ定義を存在させることはできません。
   - モダン API には`oauth2`を、シンプルなトークン認証には`apiKey`を使用し、内部/レガシーシステムの場合のみ`basic`認証を検討してください。
   - 各セキュリティ定義は正確に 1 つのタイプでなければなりません（この制約は oneOf 検証によって強制されます）。

9. **パラメータのベストプラクティス**:

   - ユーザーが各パラメータの目的を理解できるよう、説明的な`description`フィールドを使用してください。
   - より良いユーザーエクスペリエンスのために`x-ms-summary`を実装してください（タイトルケースが必要です）。
   - 適切な検証を確保するために必須パラメータを正しくマークしてください。
   - 適切なデータハンドリングを可能にするため、適切な`format`値（Power Platform 拡張機能を含む）を使用してください。
   - より良いユーザーエクスペリエンスとデータ検証のために動的拡張機能を活用してください。

10. **Power Platform 形式拡張機能**:

- `date-no-tz`: タイムオフセット情報がない日時を表します。
- `html`: クライアントに編集時は HTML エディタを、表示時は HTML ビューアを発行するよう指示します。
- 標準形式には以下が含まれます: `int32`、`int64`、`float`、`double`、`byte`、`binary`、`date`、`date-time`、`password`、`email`、`uri`、`uuid`。

### API プロパティを扱う場合

1. **接続パラメータ**:

   - `string`、`securestring`、`oauthSetting`などの適切なパラメータタイプを選択してください。
   - 正しい ID プロバイダーで OAuth 設定を構成してください。
   - 適切な場合はドロップダウンオプションに`allowedValues`を使用してください。
   - 条件付きパラメータに必要な場合はパラメータ依存関係を実装してください。

2. **ポリシーテンプレート**:

   - 異なる API エンドポイントへのバックエンドルーティングに`routerequesttoendpoint`を使用してください。
   - クエリパラメータにデフォルト値を設定するために`setqueryparameter`を実装してください。
   - ページングを正しく処理するページングシナリオで`updatenextlink`を使用してください。
   - ポーリング動作が必要なトリガー操作に`pollingtrigger`を適用してください。

3. **ブランディングとメタデータ**:
   - すべてのコネクタで必須プロパティであるため、常に`iconBrandColor`を指定してください。
   - コネクタがアクションやトリガーをサポートするかどうかを指定するため適切な`capabilities`を定義してください。
   - コネクタの所有権を識別するため意味のある`publisher`と`stackOwner`値を設定してください。

### 設定を扱う場合

1. **環境構成**:

   - 検証パターンに一致する`environment`に適切な GUID 形式を使用してください。
   - ターゲット環境に正しい`powerAppsUrl`と`flowUrl`を設定してください。
   - 特定の要件に API バージョンを一致させてください。

2. **ファイル参照**:
   - デフォルトの`apiProperties.json`と`apiDefinition.swagger.json`との一貫したファイル命名を維持してください。
   - ローカル開発環境では相対パスを使用してください。
   - アイコンファイルが存在し、構成で適切に参照されていることを確認してください。

## スキーマ検証ルール

### 必須プロパティ

- **API 定義**: `swagger: "2.0"`、`info`（`title`と`version`を含む）、`paths`
- **API プロパティ**: `iconBrandColor`を含む`properties`
- **設定**: 必須プロパティなし（すべてデフォルト値でオプション）

### パターン検証

- **ベンダー拡張機能**: Microsoft 以外の拡張機能は`^x-(?!ms-)`パターンに一致する必要があります
- **パス項目**: API パスは`/`で始まる必要があります
- **環境 GUID**: UUID 形式パターン`^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$`に一致する必要があります
- **URL**: エンドポイント構成に有効な URI である必要があります
- **ホストパターン**: `^[^{}/ :\\\\]+(?::\\d+)?$`に一致する必要があります（スペース、プロトコル、パスなし）

### タイプ制約

- **セキュリティ定義**:
  - `securityDefinitions`オブジェクトで最大 2 つのセキュリティ定義が許可されます
  - 各個別のセキュリティ定義は正確に 1 つのタイプである必要があります（oneOf 検証: `basic`、`apiKey`、`oauth2`）
  - **例外**: "None"認証は他のセキュリティ定義と共存できません
- **パラメータタイプ**: 特定の enum 値に制限されます（`string`、`number`、`integer`、`boolean`、`array`、`file`）
- **ポリシーテンプレート**: タイプ固有のパラメータ要件
- **形式値**: Power Platform 形式を含む拡張セット
- **可視性値**: `important`、`advanced`、`internal`のいずれかである必要があります
- **トリガータイプ**: `batch`または`single`である必要があります

### 追加検証ルール

- **$ref 参照**: `#/definitions/`、`#/parameters/`、`#/responses/`のみを指すべきです
- **パスパラメータ**: `required: true`としてマークする必要があります
- **Info オブジェクト**: 説明はタイトルと異なるべきです
- **Contact オブジェクト**: メールは有効なメール形式、URL は有効な URI である必要があります
- **License オブジェクト**: 名前が必要、URL は提供される場合有効な URI である必要があります
- **外部ドキュメント**: URL が必要で有効な URI である必要があります
- **タグ**: 配列内でユニークな名前である必要があります
- **スキーム**: 有効な HTTP スキーム（`http`、`https`、`ws`、`wss`）である必要があります
- **MIME タイプ**: `consumes`と`produces`で有効な MIME タイプ形式に従う必要があります

## 一般的なパターンと例

### API 定義の例

#### Microsoft 拡張機能を含む基本操作

```json
{
  "get": {
    "operationId": "GetItems",
    "summary": "Get items",
    "x-ms-summary": "Get Items",
    "x-ms-visibility": "important",
    "description": "Retrieves a list of items from the API",
    "parameters": [
      {
        "name": "category",
        "in": "query",
        "type": "string",
        "x-ms-summary": "Category",
        "x-ms-visibility": "important",
        "x-ms-dynamic-values": {
          "operationId": "GetCategories",
          "value-path": "id",
          "value-title": "name"
        }
      }
    ],
    "responses": {
      "200": {
        "description": "Success",
        "x-ms-summary": "Success",
        "schema": {
          "type": "object",
          "properties": {
            "items": {
              "type": "array",
              "x-ms-summary": "Items",
              "items": {
                "$ref": "#/definitions/Item"
              }
            }
          }
        }
      }
    }
  }
}
```

#### トリガー操作構成

```json
{
  "get": {
    "operationId": "WhenItemCreated",
    "x-ms-summary": "When an Item is Created",
    "x-ms-trigger": "batch",
    "x-ms-trigger-hint": "To see it work now, create an item",
    "x-ms-trigger-metadata": {
      "kind": "query",
      "mode": "polling"
    },
    "x-ms-pageable": {
      "nextLinkName": "@odata.nextLink"
    }
  }
}
```

#### 動的スキーマの例

```json
{
  "name": "dynamicSchema",
  "in": "body",
  "schema": {
    "x-ms-dynamic-schema": {
      "operationId": "GetSchema",
      "parameters": {
        "table": {
          "parameter": "table"
        }
      },
      "value-path": "schema"
    }
  }
}
```

#### ファイルピッカー機能

```json
{
  "x-ms-capabilities": {
    "file-picker": {
      "open": {
        "operationId": "OneDriveFilePickerOpen",
        "parameters": {
          "dataset": {
            "value-property": "dataset"
          }
        }
      },
      "browse": {
        "operationId": "OneDriveFilePickerBrowse",
        "parameters": {
          "dataset": {
            "value-property": "dataset"
          }
        }
      },
      "value-title": "DisplayName",
      "value-collection": "value",
      "value-folder-property": "IsFolder",
      "value-media-property": "MediaType"
    }
  }
}
```

#### テスト接続機能（注意: カスタムコネクタではサポートされていません）

```json
{
  "x-ms-capabilities": {
    "testConnection": {
      "operationId": "TestConnection",
      "parameters": {
        "param1": "literal-value"
      }
    }
  }
}
```

#### シミュレーション用の操作コンテキスト

```json
{
  "x-ms-operation-context": {
    "simulate": {
      "operationId": "SimulateOperation",
      "parameters": {
        "param1": {
          "parameter": "inputParam"
        }
      }
    }
  }
}
```

### 基本 OAuth 構成

```json
{
  "type": "oauthSetting",
  "oAuthSettings": {
    "identityProvider": "oauth2",
    "clientId": "your-client-id",
    "scopes": ["scope1", "scope2"],
    "redirectMode": "Global"
  }
}
```

#### 複数セキュリティ定義の例

```json
{
  "securityDefinitions": {
    "oauth2": {
      "type": "oauth2",
      "flow": "accessCode",
      "authorizationUrl": "https://api.example.com/oauth/authorize",
      "tokenUrl": "https://api.example.com/oauth/token",
      "scopes": {
        "read": "Read access",
        "write": "Write access"
      }
    },
    "apiKey": {
      "type": "apiKey",
      "name": "X-API-Key",
      "in": "header"
    }
  }
}
```

**注意**: 最大 2 つのセキュリティ定義が共存可能ですが、"None"認証は他の方法と組み合わせることができません。

### 動的パラメータ設定

```json
{
  "x-ms-dynamic-values": {
    "operationId": "GetItems",
    "value-path": "id",
    "value-title": "name"
  }
}
```

### ルーティング用ポリシーテンプレート

```json
{
  "templateId": "routerequesttoendpoint",
  "title": "Route to backend",
  "parameters": {
    "x-ms-apimTemplate-operationName": ["GetData"],
    "x-ms-apimTemplateParameter.newPath": "/api/v2/data"
  }
}
```

## ベストプラクティス

1. **IntelliSense を使用する**: これらのスキーマは開発中に役立つ豊富な自動補完と検証機能を提供します。
2. **命名規約に従う**: コードの読みやすさを向上させるため、操作とパラメータに説明的な名前を使用してください。
3. **エラーハンドリングを実装する**: 失敗シナリオを適切に処理するため、適切なレスポンススキーマとエラーコードを定義してください。
4. **十分にテストする**: 開発プロセスの早い段階で問題を発見するため、デプロイ前にスキーマを検証してください。
5. **拡張機能を文書化する**: チームの理解と将来のメンテナンスのために Microsoft 固有の拡張機能にコメントしてください。
6. **バージョン管理**: 変更と互換性を追跡するために API info でセマンティックバージョニングを使用してください。
7. **セキュリティ第一**: API エンドポイントを保護するため、常に適切な認証メカニズムを実装してください。

## トラブルシューティング

### 一般的なスキーマ違反

- **必須プロパティの不足**: `swagger: "2.0"`、`info.title`、`info.version`、`paths`
- **無効なパターン形式**:
  - GUID は正確な形式`^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$`に一致する必要があります
  - URL は適切なスキームを含む有効な URI である必要があります
  - パスは`/`で始まる必要があります
  - ホストにプロトコル、パス、スペースを含めてはいけません
- **ベンダー拡張機能の命名が不正**: Microsoft 拡張機能には`x-ms-*`を、その他には`^x-(?!ms-)`を使用してください
- **セキュリティ定義タイプの不一致**: 各セキュリティ定義は正確に 1 つのタイプである必要があります
- **無効な enum 値**: `x-ms-visibility`、`x-ms-trigger`、パラメータタイプの許可値を確認してください
- **無効な場所を指す$ref**: `#/definitions/`、`#/parameters/`、`#/responses/`を指す必要があります
- **必須としてマークされていないパスパラメータ**: すべてのパスパラメータは`required: true`である必要があります
- **間違ったコンテキストでの'file'タイプ**: スキーマではなく`formData`パラメータでのみ許可されます

### API 定義固有の問題

- **動的スキーマの競合**: `x-ms-dynamic-schema`を固定スキーマプロパティと一緒に使用できません
- **トリガー構成エラー**: `x-ms-trigger-metadata`には`kind`と`mode`の両方が必要です
- **ページング設定**: `x-ms-pageable`には`nextLinkName`プロパティが必要です
- **ファイルピッカーの設定ミス**: `open`操作と必要なプロパティの両方を含める必要があります
- **機能の競合**: 一部の機能は特定のパラメータタイプと競合する場合があります
- **テスト値のセキュリティ**: `x-ms-test-value`に秘密や PII を含めないでください
- **操作コンテキスト設定**: `x-ms-operation-context`には`operationId`を含む`simulate`オブジェクトが必要です
- **通知コンテンツスキーマ**: パスレベルの`x-ms-notification-content`は適切なスキーマ構造を定義する必要があります
- **メディア種類の制限**: `x-ms-media-kind`は`image`または`audio`値のみをサポートします
- **トリガー値構成**: `x-ms-trigger-value`には少なくとも 1 つのプロパティ（`value-collection`または`value-path`）が必要です

### 検証ツール

- コンプライアンスのためにスキーマ定義をチェックする JSON スキーマバリデーターを使用してください。
- 開発中にエラーを発見するために VS Code の組み込みスキーマ検証を活用してください。
- デプロイ前に paconn CLI でテストしてください: `paconn validate --api-def apiDefinition.swagger.json`
- 互換性を確保するために Power Platform コネクタ要件に対して検証してください。
- ターゲット環境での検証とテストに Power Platform コネクタポータルを使用してください。
- ランタイムエラーを防ぐため操作レスポンスが期待されるスキーマと一致することを確認してください。

覚えておいてください: これらのスキーマは、あなたの Power Platform コネクタが適切にフォーマットされ、Power Platform エコシステムで正しく動作することを保証します。
