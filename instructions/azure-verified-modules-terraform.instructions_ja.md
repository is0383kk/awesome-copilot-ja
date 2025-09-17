---
description: 'Azure Verified Modules (AVM) と Terraform'
applyTo: '**/*.terraform, **/*.tf, **/*.tfvars, **/*.tfstate, **/*.tflint.hcl, **/*.tf.json, **/*.tfvars.json'
---

# Azure Verified Modules (AVM) Terraform

## 概要

Azure Verified Modules (AVM) は、Azureのベストプラクティスに従った事前に構築され、テスト済み、検証済みのTerraformとBicepモジュールです。これらのモジュールを使用して、Azure Infrastructure as Code (IaC) を信頼性を持って作成、更新、レビューしてください。

## GitHub Copilot Agentsのためのカスタム指示

**重要**: GitHub Copilot AgentまたはGitHub Copilot Coding Agentがこのリポジトリで作業する際、以下のローカルユニットテストをPRチェックに準拠するために必ず実行してください。これらのテストの実行を怠ると、PR検証の失敗を引き起こします：

```bash
./avm pre-commit
./avm tflint
./avm pr-check
```

これらのコマンドは、Azure Verified Modulesの標準に準拠し、CI/CDパイプラインの失敗を防ぐために、プルリクエストを作成または更新する前に実行する必要があります。
AVMプロセスの詳細については、[Azure Verified Modules Contribution documentation](https://azure.github.io/Azure-Verified-Modules/contributing/terraform/testing/)を参照してください。

**これらのテストの実行を怠ると、PR検証の失敗を引き起こし、正常なマージを妨げます。**

## モジュールの発見

### Terraform Registry

- "avm" + リソース名で検索
- 公式AVMモジュールを見つけるために"Partner"タグでフィルター
- 例：「avm storage account」で検索 → Partnerでフィルター

### 公式AVMインデックス

- **Terraformリソース**: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-resource-modules/`
- **Terraformパターン**: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-pattern-modules/`
- **Bicepリソース**: `https://azure.github.io/Azure-Verified-Modules/indexes/bicep/bicep-resource-modules/`
- **Bicepパターン**: `https://azure.github.io/Azure-Verified-Modules/indexes/bicep/bicep-pattern-modules/`

## Terraformモジュールの使用

### サンプルからの使用

1. モジュールドキュメンテーションからサンプルコードをコピー
2. `source = "../../"` を `source = "Azure/avm-res-{service}-{resource}/azurerm"` に置換
3. `version = "~> 1.0"`を追加（利用可能な最新版を使用）
4. `enable_telemetry = true`を設定

### ゼロからの作成

1. モジュールドキュメンテーションからProvisioning Instructionsをコピー
2. 必須とオプションの入力を設定
3. モジュールバージョンを固定
4. テレメトリを有効化

### 使用例

```hcl
module "storage_account" {
  source  = "Azure/avm-res-storage-storageaccount/azurerm"
  version = "~> 0.1"

  enable_telemetry    = true
  location            = "East US"
  name                = "mystorageaccount"
  resource_group_name = "my-rg"

  # 追加の設定...
}
```

## 命名規則

### モジュールタイプ

- **リソースモジュール**: `Azure/avm-res-{service}-{resource}/azurerm`
  - 例: `Azure/avm-res-storage-storageaccount/azurerm`
- **パターンモジュール**: `Azure/avm-ptn-{pattern}/azurerm`
  - 例: `Azure/avm-ptn-aks-enterprise/azurerm`
- **ユーティリティモジュール**: `Azure/avm-utl-{utility}/azurerm`
  - 例: `Azure/avm-utl-regions/azurerm`

### サービス命名

- サービスとリソースにはkebab-caseを使用
- Azureサービス名に従う（例：`storage-storageaccount`, `network-virtualnetwork`）

## バージョン管理

### 利用可能なバージョンの確認

- エンドポイント: `https://registry.terraform.io/v1/modules/Azure/{module}/azurerm/versions`
- 例: `https://registry.terraform.io/v1/modules/Azure/avm-res-storage-storageaccount/azurerm/versions`

### バージョン固定のベストプラクティス

- 悲観的バージョン制約を使用: `version = "~> 1.0"`
- 本番環境では特定のバージョンに固定: `version = "1.2.3"`
- アップグレード前は常にchangelogを確認

## モジュールのソース

### Terraform Registry

- **URLパターン**: `https://registry.terraform.io/modules/Azure/{module}/azurerm/latest`
- **例**: `https://registry.terraform.io/modules/Azure/avm-res-storage-storageaccount/azurerm/latest`

### GitHubリポジトリ

- **URLパターン**: `https://github.com/Azure/terraform-azurerm-avm-{type}-{service}-{resource}`
- **例**:
  - リソース: `https://github.com/Azure/terraform-azurerm-avm-res-storage-storageaccount`
  - パターン: `https://github.com/Azure/terraform-azurerm-avm-ptn-aks-enterprise`

## 開発のベストプラクティス

### モジュール使用

- ✅ **常に** モジュールとプロバイダーのバージョンを固定
- ✅ **開始時は** モジュールドキュメンテーションの公式サンプルを使用
- ✅ **実装前に** すべての入力と出力を確認
- ✅ **テレメトリを有効化**: `enable_telemetry = true`
- ✅ **共通パターンには** AVMユーティリティモジュールを使用
- ✅ **AzureRMプロバイダーの要件と制約に従う**

### コード品質

- ✅ **常に** 変更後に `terraform fmt` を実行
- ✅ **常に** 変更後に `terraform validate` を実行
- ✅ **意味のある変数名と説明を使用**
- ✅ **適切なタグとメタデータを追加**
- ✅ **複雑な設定を文書化**

### 検証要件

プルリクエストを作成または更新する前に：

```bash
# コードをフォーマット
terraform fmt -recursive

# 構文を検証
terraform validate

# AVM固有の検証（必須）
./avm pre-commit
./avm tflint
./avm pr-check
```

## ツール統合

### 利用可能なツールの使用

- **デプロイメントガイダンス**: `azure_get_deployment_best_practices` ツールを使用
- **サービスドキュメンテーション**: Azureサービス固有のガイダンスに `microsoft.docs.mcp` ツールを使用
- **スキーマ情報**: Bicepリソースには `azure_get_schema_for_Bicep` を使用

### GitHub Copilot統合

AVMリポジトリで作業する際：

1. 新しいリソースを作成する前に既存のモジュールをチェック
2. 開始点として公式サンプルを使用
3. コミット前にすべての検証テストを実行
4. カスタマイゼーションやサンプルからの逸脱を文書化

## 一般的なパターン

### リソースグループモジュール

```hcl
module "resource_group" {
  source  = "Azure/avm-res-resources-resourcegroup/azurerm"
  version = "~> 0.1"

  enable_telemetry = true
  location         = var.location
  name            = var.resource_group_name
}
```

### Virtual Networkモジュール

```hcl
module "virtual_network" {
  source  = "Azure/avm-res-network-virtualnetwork/azurerm"
  version = "~> 0.1"

  enable_telemetry    = true
  location            = module.resource_group.location
  name                = var.vnet_name
  resource_group_name = module.resource_group.name
  address_space       = ["10.0.0.0/16"]
}
```

## トラブルシューティング

### 一般的な問題

1. **バージョン競合**: モジュールとプロバイダーのバージョン間の互換性を常に確認
2. **依存関係の欠如**: すべての必要なリソースが最初に作成されることを確認
3. **検証の失敗**: コミット前にAVM検証ツールを実行
4. **ドキュメンテーション**: 常に最新のモジュールドキュメンテーションを参照

### サポートリソース

- **AVMドキュメンテーション**: `https://azure.github.io/Azure-Verified-Modules/`
- **GitHub Issues**: 特定のモジュールのGitHubリポジトリで問題を報告
- **コミュニティ**: Azure Terraform Provider GitHubディスカッション

## コンプライアンスチェックリスト

AVM関連コードを提出する前に：

- [ ] モジュールバージョンが固定されている
- [ ] テレメトリが有効化されている
- [ ] コードがフォーマットされている（`terraform fmt`）
- [ ] コードが検証されている（`terraform validate`）
- [ ] AVM pre-commitチェックが通過している（`./avm pre-commit`）
- [ ] TFLintチェックが通過している（`./avm tflint`）
- [ ] AVM PRチェックが通過している（`./avm pr-check`）
- [ ] ドキュメンテーションが更新されている
- [ ] サンプルがテスト済みで動作している
