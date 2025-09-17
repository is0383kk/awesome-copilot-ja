---
description: 'Workflow Definition Language (WDL)、統合パターン、エンタープライズオートメーションのベストプラクティスを用いたAzure Logic AppsとPower Automateワークフローの開発ガイドライン'
applyTo: "**/*.json,**/*.logicapp.json,**/workflow.json,**/*-definition.json,**/*.flow.json"
---

# Azure Logic Apps and Power Automateの指示

## 概要

これらの指示は、JSONベースのワークフロー定義言語（WDL）を使用してハイクオリティなAzure Logic AppsとMicrosoft Power Automateのワークフロー定義を書くためのガイドです。Azure Logic Appsはサービスとプロトコル間の統合を簡素化する1,400以上のコネクターを提供するクラウドベースのIntegration Platform as a Service (iPaaS)です。これらのガイドラインに従って、堅牢で効率的で保守可能なクラウドワークフローオートメーションソリューションを作成してください。

## ワークフロー定義言語の構造

Logic AppsまたはPower AutomateのフローJSONファイルを扱う際は、ワークフローが以下の標準構造に従うようにしてください：

```json
{
  "definition": {
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "actions": { },
    "contentVersion": "1.0.0.0",
    "outputs": { },
    "parameters": { },
    "staticResults": { },
    "triggers": { }
  },
  "parameters": { }
}
```

## Azure Logic AppsとPower Automate開発のベストプラクティス

### 1. トリガー

- **シナリオに基づいて適切なトリガータイプを使用**：
  - **リクエストトリガー**: 同期的なAPI的なワークフロー用
  - **定期実行トリガー**: スケジュールされた操作用
  - **イベントベーストリガー**: リアクティブパターン用（Service Bus、Event Gridなど）
- **適切なトリガー設定の構成**：
  - 適切なタイムアウト期間を設定
  - 大量データソース用のページネーション設定を使用
  - 適切な認証を実装

```json
"triggers": {
  "manual": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
      "schema": {
        "type": "object",
        "properties": {
          "requestParameter": {
            "type": "string"
          }
        }
      }
    }
  }
}
```

### 2. アクション

- **アクションを目的を示すよう記述的に名前を付ける**
- **複雑なワークフローを論理的なグループ化でスコープを使って整理する**
- **異なる操作に適切なアクションタイプを使用する**：
  - API呼び出し用のHTTPアクション
  - 組み込み統合用のコネクターアクション
  - 変換用のデータ操作アクション

```json
"actions": {
  "Get_Customer_Data": {
    "type": "Http",
    "inputs": {
      "method": "GET",
      "uri": "https://api.example.com/customers/@{triggerBody()?['customerId']}",
      "headers": {
        "Content-Type": "application/json"
      }
    },
    "runAfter": {}
  }
}
```

### 3. エラーハンドリングと信頼性

- **堅牢なエラーハンドリングを実装する**：
  - 失敗を処理するため"runAfter"設定を使用
  - 一時的エラー用の再試行ポリシーを設定
  - エラーブランチ用の"runAfter"条件でスコープを使用
- **重要な操作にフォールバックメカニズムを実装する**
- **外部サービス呼び出しにタイムアウトを追加する**
- **複雑なエラーハンドリングシナリオにrunAfter条件を使用する**

```json
"actions": {
  "HTTP_Action": {
    "type": "Http",
    "inputs": { },
    "retryPolicy": {
      "type": "fixed",
      "count": 3,
      "interval": "PT20S",
      "minimumInterval": "PT5S",
      "maximumInterval": "PT1H"
    }
  },
  "Handle_Success": {
    "type": "Scope",
    "actions": { },
    "runAfter": {
      "HTTP_Action": ["Succeeded"]
    }
  },
  "Handle_Failure": {
    "type": "Scope",
    "actions": {
      "Log_Error": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['loganalytics']['connectionId']"
            }
          },
          "method": "post",
          "body": {
            "LogType": "WorkflowError",
            "ErrorDetails": "@{actions('HTTP_Action').outputs.body}",
            "StatusCode": "@{actions('HTTP_Action').outputs.statusCode}"
          }
        }
      },
      "Send_Notification": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['office365']['connectionId']"
            }
          },
          "method": "post",
          "path": "/v2/Mail",
          "body": {
            "To": "support@contoso.com",
            "Subject": "Workflow Error - HTTP Call Failed",
            "Body": "<p>The HTTP call failed with status code: @{actions('HTTP_Action').outputs.statusCode}</p>"
          }
        },
        "runAfter": {
          "Log_Error": ["Succeeded"]
        }
      }
    },
    "runAfter": {
      "HTTP_Action": ["Failed", "TimedOut"]
    }
  }
}
```

### 4. 式と関数

- **データを変換するため組み込み式関数を使用する**
- **式を簡潔で読みやすく保つ**
- **複雑な式をコメントで文書化する**

一般的な式パターン:
- 文字列操作: `concat()`, `replace()`, `substring()`
- コレクション操作: `filter()`, `map()`, `select()`
- 条件論理: `if()`, `and()`, `or()`, `equals()`
- 日時操作: `formatDateTime()`, `addDays()`
- JSON処理: `json()`, `array()`, `createArray()`

```json
"Set_Variable": {
  "type": "SetVariable",
  "inputs": {
    "name": "formattedData",
    "value": "@{map(body('Parse_JSON'), item => {
      return {
        id: item.id,
        name: toUpper(item.name),
        date: formatDateTime(item.timestamp, 'yyyy-MM-dd')
      }
    })}"
  }
}
```

#### Power Automate条件での式の使用

Power Automateは条件で複数の値をチェックする高度な式をサポートします。複雑な論理条件を扱う際は、以下のパターンを使用してください：

- 単一の値を比較する場合: 基本的な条件デザイナーインターフェースを使用
- 複数の条件の場合: 高度モードで高度な式を使用

Power Automateの条件での一般的な論理式関数：

| 式 | 説明 | 例 |
|------------|-------------|---------|
| `and` | 両方の引数がtrueの場合trueを返す | `@and(equals(item()?['Status'], 'completed'), equals(item()?['Assigned'], 'John'))` |
| `or` | いずれかの引数がtrueの場合trueを返す | `@or(equals(item()?['Status'], 'completed'), equals(item()?['Status'], 'unnecessary'))` |
| `equals` | 値が等しいかチェック | `@equals(item()?['Status'], 'blocked')` |
| `greater` | 最初の値が2番目より大きいかチェック | `@greater(item()?['Due'], item()?['Paid'])` |
| `less` | 最初の値が2番目より小さいかチェック | `@less(item()?['dueDate'], addDays(utcNow(),1))` |
| `empty` | オブジェクト、配列、文字列が空かチェック | `@empty(item()?['Status'])` |
| `not` | ブール値の逆を返す | `@not(contains(item()?['Status'], 'Failed'))` |

例: ステータスが"completed"または"unnecessary"かチェック：
```
@or(equals(item()?['Status'], 'completed'), equals(item()?['Status'], 'unnecessary'))
```

例: ステータスが"blocked"かつ特定の人にアサインされているかチェック：
```
@and(equals(item()?['Status'], 'blocked'), equals(item()?['Assigned'], 'John Wonder'))
```

例: 支払いが期限切れかつ未完了かチェック：
```
@and(greater(item()?['Due'], item()?['Paid']), less(item()?['dueDate'], utcNow()))
```

**注意:** Power Automateでは、式で前のステップからの動的値にアクセスする際、コレクション内のプロパティに安全にアクセスするために`item()?['PropertyName']`構文を使用してください。

### 5. パラメーターと変数

- **環境間での再利用性のためワークフローをパラメーター化する**
- **ワークフロー内の一時的な値に変数を使用する**
- **デフォルト値と説明を含む明確なパラメータースキーマを定義する**

```json
"parameters": {
  "apiEndpoint": {
    "type": "string",
    "defaultValue": "https://api.dev.example.com",
    "metadata": {
      "description": "The base URL for the API endpoint"
    }
  }
},
"variables": {
  "requestId": "@{guid()}",
  "processedItems": []
}
```

### 6. 制御フロー

- **分岐ロジックに条件を使用する**
- **独立した操作に並列ブランチを実装する**
- **コレクション用に適切なバッチサイズでforeachループを使用する**
- **適切な終了条件でuntilループを適用する**

```json
"Process_Items": {
  "type": "Foreach",
  "foreach": "@body('Get_Items')",
  "actions": {
    "Process_Single_Item": {
      "type": "Scope",
      "actions": { }
    }
  },
  "runAfter": {
    "Get_Items": ["Succeeded"]
  },
  "runtimeConfiguration": {
    "concurrency": {
      "repetitions": 10
    }
  }
}
```

### 7. コンテンツとメッセージの処理

- **データ整合性を確保するためメッセージスキーマを検証する**
- **適切なコンテンツタイプの処理を実装する**
- **構造化データを扱うためParse JSONアクションを使用する**

```json
"Parse_Response": {
  "type": "ParseJson",
  "inputs": {
    "content": "@body('HTTP_Request')",
    "schema": {
      "type": "object",
      "properties": {
        "id": {
          "type": "string"
        },
        "data": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": { }
          }
        }
      }
    }
  }
}
```

### 8. セキュリティのベストプラクティス

- **可能な限りマネージドアイデンティティを使用する**
- **秘密情報をKey Vaultに保存する**
- **接続に最小権限アクセスを実装する**
- **認証でAPIエンドポイントを保護する**
- **HTTPトリガーにIP制限を実装する**
- **パラメーターとメッセージ内の機密データにデータ暗号化を適用する**
- **Logic Appsリソースへのアクセス制御にAzure RBACを使用する**
- **ワークフローと接続の定期的なセキュリティレビューを実施する**

```json
"Get_Secret": {
  "type": "ApiConnection",
  "inputs": {
    "host": {
      "connection": {
        "name": "@parameters('$connections')['keyvault']['connectionId']"
      }
    },
    "method": "get",
    "path": "/secrets/@{encodeURIComponent('apiKey')}/value"
  }
},
"Call_Protected_API": {
  "type": "Http",
  "inputs": {
    "method": "POST",
    "uri": "https://api.example.com/protected",
    "headers": {
      "Content-Type": "application/json",
      "Authorization": "Bearer @{body('Get_Secret')?['value']}"
    },
    "body": {
      "data": "@variables('processedData')"
    }
  },
  "authentication": {
    "type": "ManagedServiceIdentity"
  },
  "runAfter": {
    "Get_Secret": ["Succeeded"]
  }
}
```

## パフォーマンス最適化

- **不要なアクションを最小限に抑える**
- **利用可能な場合はバッチ操作を使用する**
- **複雑性を減らすため式を最適化する**
- **適切なタイムアウト値を設定する**
- **大きなデータセットにページネーションを実装する**
- **並列化可能な操作に並行制御を実装する**

```json
"Process_Items": {
  "type": "Foreach",
  "foreach": "@body('Get_Items')",
  "actions": {
    "Process_Single_Item": {
      "type": "Scope",
      "actions": { }
    }
  },
  "runAfter": {
    "Get_Items": ["Succeeded"]
  },
  "runtimeConfiguration": {
    "concurrency": {
      "repetitions": 10
    }
  }
}
```

### ワークフロー設計のベストプラクティス

- **最適なデザイナーパフォーマンスのためワークフローを50アクション以下に制限する**
- **必要に応じて複雑なビジネスロジックを複数の小さなワークフローに分割する**
- **ゼロダウンタイムデプロイメントが必要なミッションクリティカルなロジックアプリにはデプロイメントスロットを使用する**
- **トリガーとアクション定義でハードコーディングされたプロパティを避ける**
- **トリガーとアクション定義に関するコンテキストを提供するため記述的コメントを追加する**
- **より良いパフォーマンスのため利用可能な場合は共有コネクターの代わりに組み込み操作を使用する**
- **B2BシナリオとEDIメッセージ処理にはIntegration Accountを使用する**
- **組織内で標準パターンのワークフローテンプレートを再利用する**
- **可読性を維持するためスコープとアクションの深いネストを避ける**

### モニタリングと可観測性

- **ワークフロー実行とメトリクスをキャプチャするため診断設定を構成する**
- **関連するワークフロー実行を関連付けるためトラッキングIDを追加する**
- **適切な詳細レベルで包括的なログ記録を実装する**
- **ワークフロー障害とパフォーマンス低下にアラートを設定する**
- **エンドツーエンドのトレースとモニタリングにApplication Insightsを使用する**

## プラットフォームタイプと考慮事項

### Azure Logic Apps vs Power Automate

Azure Logic AppsとPower Automateは同じ基盤ワークフローエンジンと言語を共有していますが、異なるターゲット読者と機能を持っています：

- **Power Automate**：
  - ビジネスユーザー向けのユーザーフレンドリーなインターフェース
  - Power Platformエコシステムの一部
  - Microsoft 365とDynamics 365との統合
  - UI自動化のためのデスクトップフロー機能

- **Azure Logic Apps**：
  - エンタープライズグレードの統合プラットフォーム
  - 高度な機能を持つ開発者向け
  - より深いAzureサービス統合
  - より豊富なモニタリングと運用機能

### Logic Appの種類

#### Consumption Logic Apps
- 実行毎の従量制価格モデル
- サーバーレスアーキテクチャ
- 変動またはして予測不可能なワークロードに適している

#### Standard Logic Apps
- App Service Planに基づく固定価格
- 予測可能なパフォーマンス
- ローカル開発サポート
- VNetsとの統合

#### Integration Service Environment (ISE)
- 専用デプロイメント環境
- より高いスループットとより長い実行時間
- VNetリソースへの直接アクセス
- 分離されたランタイム環境

### Power Automateライセンスタイプ
- **Power Automate per userプラン**: 個人ユーザー用
- **Power Automate per flowプラン**: 特定のワークフロー用
- **Power Automate Processプラン**: RPA機能用
- **Office 365に含まれるPower Automate**: Office 365ユーザー向けの制限された機能

## 一般的な統合パターン

### アーキテクチャパターン
- **メディエーターパターン**: システム間のオーケストレーション層としてLogic Apps/Power Automateを使用
- **コンテンツベースルーティング**: コンテンツに基づいてメッセージを異なる宛先にルーティング
- **メッセージ変換**: フォーマット間でのメッセージ変換（JSON、XML、EDIなど）
- **分散収集**: 作業を並列に分散し、結果を集約
- **プロトコルブリッジング**: 異なるプロトコル（REST、SOAP、FTPなど）でシステムを接続
- **クレームチェック**: blobストレージやデータベースに大きなペイロードを外部保存
- **Sagaパターン**: 失敗時の補償アクションで分散トランザクションを管理
- **コレオグラフィパターン**: 中央オーケストレーターなしで複数のサービスを調整

### アクションパターン
- **非同期処理パターン**: 長時間実行操作用
  ```json
  "LongRunningAction": {
    "type": "Http",
    "inputs": {
      "method": "POST",
      "uri": "https://api.example.com/longrunning",
      "body": { "data": "@triggerBody()" }
    },
    "retryPolicy": {
      "type": "fixed",
      "count": 3,
      "interval": "PT30S"
    }
  }
  ```

- **Webhookパターン**: コールバックベースの処理用
  ```json
  "WebhookAction": {
    "type": "ApiConnectionWebhook",
    "inputs": {
      "host": {
        "connection": {
          "name": "@parameters('$connections')['servicebus']['connectionId']"
        }
      },
      "body": {
        "content": "@triggerBody()"
      },
      "path": "/subscribe/topics/@{encodeURIComponent('mytopic')}/subscriptions/@{encodeURIComponent('mysubscription')}"
    }
  }
  ```

### エンタープライズ統合パターン
- **B2Bメッセージ交換**: 取引パートナー間でのEDI文書の交換（AS2、X12、EDIFACT）
- **Integration Account**: B2Bアーティファクト（合意、スキーマ、マップ）の保存と管理に使用
- **ルールエンジン**: Azure Logic Apps Rules Engineを使用した複雑なビジネスルールの実装
- **メッセージ検証**: コンプライアンスとデータ整合性のためのスキーマに対するメッセージ検証
- **トランザクション処理**: ロールバック用の補償トランザクションでのビジネストランザクション処理

## Logic AppsのDevOpsとCI/CD

### ソース管理とバージョニング

- **Logic App定義をソース管理に保存する**（Git、Azure DevOps、GitHub）
- **複数環境へのデプロイメントにARMテンプレートを使用する**
- **リリース頻度に適したブランチング戦略を実装する**
- **タグやバージョンプロパティを使用してLogic Appsをバージョン管理する**

### 自動化されたデプロイメント

- **自動化デプロイメントにAzure DevOpsパイプラインまたはGitHub Actionsを使用する**
- **環境固有の値にパラメーター化を実装する**
- **ゼロダウンタイムデプロイメントにデプロイメントスロットを使用する**
- **CI/CDパイプラインにデプロイメント後の検証テストを含める**

```yaml
# Logic Appデプロイメント用のAzure DevOps YAMLパイプラインの例
trigger:
  branches:
    include:
    - main
    - release/*

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzureResourceManagerTemplateDeployment@3
  inputs:
    deploymentScope: 'Resource Group'
    azureResourceManagerConnection: 'Your-Azure-Connection'
    subscriptionId: '$(subscriptionId)'
    action: 'Create Or Update Resource Group'
    resourceGroupName: '$(resourceGroupName)'
    location: '$(location)'
    templateLocation: 'Linked artifact'
    csmFile: '$(System.DefaultWorkingDirectory)/arm-templates/logicapp-template.json'
    csmParametersFile: '$(System.DefaultWorkingDirectory)/arm-templates/logicapp-parameters-$(Environment).json'
    deploymentMode: 'Incremental'
```

## クロスプラットフォームの考慮事項

Azure Logic AppsとPower Automateの両方を扱う場合：

- **エクスポート/インポート互換性**: フローはPower AutomateからエクスポートしてLogic Appsにインポートできるが、一部の変更が必要な場合がある
- **コネクターの違い**: 一部のコネクターは片方のプラットフォームでのみ利用可能
- **環境の分離**: Power Automate環境は分離を提供し、異なるポリシーを持つ場合がある
- **ALMプラクティス**: Logic AppsにはAzure DevOpsを、Power AutomateにはSolutionsの使用を検討

### 移行戦略

- **アセスメント**: 移行の複雑性と適合性を評価
- **コネクターマッピング**: プラットフォーム間でのコネクターをマップし、ギャップを特定
- **テスト戦略**: カットオーバー前の並列テストを実装
- **ドキュメンテーション**: 参照用にすべての設定変更を文書化

```json
// Power Automateフロー用のPower Platformソリューション構造の例
{
  "SolutionName": "MyEnterpriseFlows",
  "Version": "1.0.0",
  "Flows": [
    {
      "Name": "OrderProcessingFlow",
      "Type": "Microsoft.Flow/flows",
      "Properties": {
        "DisplayName": "Order Processing Flow",
        "DefinitionData": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "triggers": {
            "When_a_new_order_is_created": {
              "type": "ApiConnectionWebhook",
              "inputs": {
                "host": {
                  "connectionName": "shared_commondataserviceforapps",
                  "operationId": "SubscribeWebhookTrigger",
                  "apiId": "/providers/Microsoft.PowerApps/apis/shared_commondataserviceforapps"
                }
              }
            }
          },
          "actions": {
            // アクションはここで定義される
          }
        }
      }
    }
  ]
}
```

## その他のリソース

- [Azure Logic Apps ドキュメンテーション](https://docs.microsoft.com/ja-jp/azure/logic-apps/)
- [Power Automate ドキュメンテーション](https://docs.microsoft.com/ja-jp/power-automate/)
- [ワークフロー定義言語スキーマ](https://docs.microsoft.com/ja-jp/azure/logic-apps/logic-apps-workflow-definition-language)
- [Power Automate vs Logic Apps比較](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs)
- [エンタープライズ統合パターン](https://docs.microsoft.com/ja-jp/azure/logic-apps/enterprise-integration-overview)
- [Logic Apps B2Bドキュメンテーション](https://docs.microsoft.com/ja-jp/azure/logic-apps/logic-apps-enterprise-integration-b2b)
- [Azure Logic Appsの制限と設定](https://docs.microsoft.com/ja-jp/azure/logic-apps/logic-apps-limits-and-config)
- [Logic Appsパフォーマンス最適化](https://docs.microsoft.com/ja-jp/azure/logic-apps/logic-apps-performance-optimization)
- [Logic Appsセキュリティ概要](https://docs.microsoft.com/ja-jp/azure/logic-apps/logic-apps-securing-a-logic-app)
- [API ManagementとLogic Apps統合](https://docs.microsoft.com/ja-jp/azure/api-management/api-management-create-api-logic-app)
- [Logic Apps Standardネットワーキング](https://docs.microsoft.com/ja-jp/azure/logic-apps/connect-virtual-network-vnet-isolated-environment)
