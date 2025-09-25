---
description: 'Jestを使用したJavaScript/TypeScriptテストの書き方のベストプラクティス（モック戦略、テスト構造、一般的なパターンを含む）'
mode: 'agent'
---

### テスト構造
- テストファイルは`.test.ts`または`.test.js`の拡張子を付けて命名する
- テストファイルをテスト対象のコードの隣に配置するか、専用の`__tests__`ディレクトリに配置する
- 期待される動作を説明する説明的なテスト名を使用する
- 関連するテストを整理するためにネストしたdescribeブロックを使用する
- パターンに従う：`describe('コンポーネント/関数/クラス', () => { it('〜すべき', () => {}) })`

### 効果的なモック
- テストを分離するために外部依存関係（API、データベースなど）をモックする
- モジュールレベルのモックには`jest.mock()`を使用する
- 特定の関数のモックには`jest.spyOn()`を使用する
- モックの動作を定義するために`mockImplementation()`または`mockReturnValue()`を使用する
- `afterEach`で`jest.resetAllMocks()`を使用してテスト間でモックをリセットする

### 非同期コードのテスト
- テストではpromiseを返すか、async/await構文を必ず使用する
- promiseには`resolves`/`rejects`マッチャーを使用する
- 遅いテストには`jest.setTimeout()`で適切なタイムアウトを設定する

### スナップショットテスト
- 頻繁に変更されないUIコンポーネントや複雑なオブジェクトにはスナップショットテストを使用する
- スナップショットを小さく焦点を絞って保持する
- コミット前にスナップショットの変更を慎重に確認する

### Reactコンポーネントのテスト
- コンポーネントのテストにはEnzymeよりもReact Testing Libraryを使用する
- ユーザーの動作とコンポーネントのアクセシビリティをテストする
- アクセシビリティの役割、ラベル、またはテキストコンテンツで要素をクエリする
- より現実的なユーザーインタラクションには`fireEvent`より`userEvent`を使用する

## 一般的なJestマッチャー
- 基本：`expect(value).toBe(expected)`、`expect(value).toEqual(expected)`
- 真偽性：`expect(value).toBeTruthy()`、`expect(value).toBeFalsy()`
- 数値：`expect(value).toBeGreaterThan(3)`、`expect(value).toBeLessThanOrEqual(3)`
- 文字列：`expect(value).toMatch(/pattern/)`、`expect(value).toContain('substring')`
- 配列：`expect(array).toContain(item)`、`expect(array).toHaveLength(3)`
- オブジェクト：`expect(object).toHaveProperty('key', value)`
- 例外：`expect(fn).toThrow()`、`expect(fn).toThrow(Error)`
- モック関数：`expect(mockFn).toHaveBeenCalled()`、`expect(mockFn).toHaveBeenCalledWith(arg1, arg2)`