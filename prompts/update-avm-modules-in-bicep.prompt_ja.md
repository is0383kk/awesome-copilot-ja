---
mode: 'agent'
description: 'BicepファイルのAzure Verified Modules (AVM) を最新バージョンに更新します。'
tools: ['codebase', 'think', 'changes', 'fetch', 'searchResults', 'todos', 'editFiles', 'search', 'runCommands', 'bicepschema', 'azure_get_schema_for_Bicep']
---
# BicepファイルのAzure Verified Modules更新

Bicepファイル `${file}` を更新して最新のAzure Verified Module（AVM）バージョンを使用します。進捗更新は非破壊的変更に制限します。最終出力テーブルとサマリー以外の情報は出力しないでください。

## プロセス

1. **スキャン**: `${file}` からAVMモジュールと現在のバージョンを抽出
1. **特定**: `#search`ツールを使用して`avm/res/{service}/{resource}`をマッチングし、使用されているすべてのユニークなAVMモジュールをリスト
1. **確認**: `#fetch`ツールを使用してMCRから各AVMモジュールの最新バージョンを取得: `https://mcr.microsoft.com/v2/bicep/avm/res/{service}/{resource}/tags/list`
1. **比較**: セマンティックバージョンを解析してアップデートが必要なAVMモジュールを特定
1. **確認**: 破壊的変更については、`#fetch`ツールを使用してドキュメントを取得: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/{service}/{resource}`
1. **更新**: `#editFiles`ツールを使用してバージョン更新とパラメーター変更を適用
1. **検証**: `#runCommands`ツールを使用して`bicep lint`と`bicep build`を実行し、コンプライアンスを確認
1. **出力**: 変更をテーブル形式で要約し、下に更新のサマリーを記述

## ツール使用

利用可能な場合は常に `#search`、`#searchResults`、`#fetch`、`#editFiles`、`#runCommands`、`#todos` ツールを使用してください。タスクを実行するためのコード記述は避けてください。

## 破壊的変更ポリシー

⚠️ 更新が以下を含む場合は**承認を待つ**：

- 非互換パラメーター変更
- セキュリティ/コンプライアンス修正
- 動作変更

## 出力形式

アイコン付きテーブルでのみ結果を表示：

```markdown
| モジュール | 現在 | 最新 | ステータス | アクション | ドキュメント |
|--------|---------|--------|--------|--------|------|
| avm/res/compute/vm | 0.1.0 | 0.2.0 | 🔄 | 更新済み | [📖](link) |
| avm/res/storage/account | 0.3.0 | 0.3.0 | ✅ | 最新 | [📖](link) |

### 更新サマリー

実行された更新、必要な手動レビューまたは遭遇した問題について説明します。
```

## アイコン

- 🔄 更新済み
- ✅ 最新
- ⚠️ 手動確認が必要
- ❌ 失敗
- 📖 ドキュメント

## 要件

- バージョン発見にはMCR tags APIのみを使用
- JSONタグ配列を解析してセマンティックバージョニングで並べ替え
- Bicepファイルの有効性とリント準拠を維持