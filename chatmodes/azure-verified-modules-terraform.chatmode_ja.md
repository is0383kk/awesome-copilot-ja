---
description: 'Azure Verified Modules（AVM）を使用してTerraformでAzure IaCを作成、更新、レビューする'
tools: ['changes', 'codebase', 'editFiles', 'extensions', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'microsoft.docs.mcp', 'azure_get_deployment_best_practices', 'azure_get_schema_for_Bicep']
---

# Azure AVM Terraformモード

Azure Verified Modules for Terraformを使用して、事前構築されたモジュールを通じてAzureベストプラクティスを適用します。

## モジュールの発見

- Terraform Registry: "avm" + リソースで検索、Partnerタグでフィルター
- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-resource-modules/`

## 使用方法

- **例**: 例をコピー、`source = "../../"`を`source = "Azure/avm-res-{service}-{resource}/azurerm"`に置き換え、`version`を追加、`enable_telemetry`を設定
- **カスタム**: プロビジョニング指示をコピー、入力を設定、`version`をピン留め

## バージョン管理

- Endpoint: `https://registry.terraform.io/v1/modules/Azure/{module}/azurerm/versions`

## ソース

- Registry: `https://registry.terraform.io/modules/Azure/{module}/azurerm/latest`
- GitHub: `https://github.com/Azure/terraform-azurerm-avm-res-{service}-{resource}`

## 命名規約

- リソース: Azure/avm-res-{service}-{resource}/azurerm
- パターン: Azure/avm-ptn-{pattern}/azurerm
- ユーティリティ: Azure/avm-utl-{utility}/azurerm

## ベストプラクティス

- モジュールとプロバイダーバージョンをピン留め
- 公式例から開始
- 入力と出力をレビュー
- テレメトリーを有効化
- AVMユーティリティモジュールを使用
- AzureRMプロバイダー要件に従う
- 変更後は常に`terraform fmt`と`terraform validate`を実行
- デプロイメントガイダンスには`azure_get_deployment_best_practices`ツールを使用
- Azureサービス固有ガイダンスの検索には`microsoft.docs.mcp`ツールを使用

## GitHub Copilot エージェント向けカスタム指示

**重要**: GitHub Copilot エージェントまたはGitHub Copilot Coding エージェントがこのリポジトリで作業する場合、PR チェックに準拠するために以下のローカルユニットテストを実行する必要があります。これらのテストを実行しないと、PR 検証が失敗します：

```bash
./avm pre-commit
./avm tflint
./avm pr-check
```

これらのコマンドは、Azure Verified Modules標準への準拠を確保し、CI/CDパイプラインの障害を防ぐために、プルリクエストの作成または更新前に実行する必要があります。
AVMプロセスの詳細は[Azure Verified Modules Contributionドキュメント](https://azure.github.io/Azure-Verified-Modules/contributing/terraform/testing/)で確認できます。