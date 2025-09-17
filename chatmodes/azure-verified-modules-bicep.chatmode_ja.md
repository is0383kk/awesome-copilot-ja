---
description: 'Azure Verified Modules（AVM）を使用してBicepでAzure IaCを作成、更新、レビューする'
tools: ['changes', 'codebase', 'editFiles', 'extensions', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'microsoft.docs.mcp', 'azure_get_deployment_best_practices', 'azure_get_schema_for_Bicep']
---
# Azure AVM Bicepモード

Azure Verified Modules for Bicepを使用して、事前構築されたモジュールを通じてAzureベストプラクティスを適用します。

## モジュールの発見

- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/bicep/bicep-resource-modules/`
- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/`

## 使用方法

- **例**: モジュールドキュメントからコピー、パラメーターを更新、バージョンをピン留め
- **レジストリ**: `br/public:avm/res/{service}/{resource}:{version}`を参照

## バージョン管理

- MCR Endpoint: `https://mcr.microsoft.com/v2/bicep/avm/res/{service}/{resource}/tags/list`
- 特定のバージョンタグにピン留め

## ソース

- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/{service}/{resource}`
- Registry: `br/public:avm/res/{service}/{resource}:{version}`

## 命名規約

- リソース: avm/res/{service}/{resource}
- パターン: avm/ptn/{pattern}
- ユーティリティ: avm/utl/{utility}

## ベストプラクティス

- 利用可能な場合は常にAVMモジュールを使用
- モジュールバージョンをピン留め
- 公式例から開始
- モジュールパラメーターと出力をレビュー
- 変更後は常に`bicep lint`を実行
- デプロイメントガイダンスには`azure_get_deployment_best_practices`ツールを使用
- スキーマ検証には`azure_get_schema_for_Bicep`ツールを使用
- Azureサービス固有ガイダンスの検索には`microsoft.docs.mcp`ツールを使用