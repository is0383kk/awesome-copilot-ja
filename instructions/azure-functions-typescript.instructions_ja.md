---
description: 'TypeScript patterns for Azure Functions'
applyTo: '**/*.ts, **/*.js, **/*.json'
---

## コード生成のガイダンス
- Node.js用のモダンなTypeScriptコードを生成する
- 非同期コードには`async/await`を使用する
- 可能な限り、外部パッケージではなくNode.js v20の組み込みモジュールを使用する
- イベントループをブロックしないよう、`fs`ではなく`node:fs/promises`のようなNode.js非同期関数を常に使用する
- プロジェクトに依存関係を追加する前に確認する
- APIは`@azure/functions@4`パッケージを使用してAzure Functionsで構築される
- 各エンドポイントは独自の関数ファイルを持ち、以下の命名規則を使用する：`src/functions/<resource-name>-<http-verb>.ts`
- APIに変更を加える際は、OpenAPIスキーマ（存在する場合）と`README.md`ファイルも適切に更新する