---
description: 'Playwrightテスト生成指示事項'
applyTo: '**'
---

## テスト記述ガイドライン

### コード品質標準
- **ロケーター**: 復元力とアクセシビリティのため、ユーザー向けでロールベースのロケーター（`getByRole`、`getByLabel`、`getByText`など）を優先する。インタラクションをグループ化し、テストの読みやすさとレポート作成を向上させるため`test.step()`を使用する。
- **アサーション**: 自動再試行するウェブファーストアサーションを使用する。これらのアサーションは`await`キーワードで始まる（例：`await expect(locator).toHaveText()`）。可視性の変化を特定にテストする場合を除き、`expect(locator).toBeVisible()`は避ける。
- **タイムアウト**: Playwrightの組み込み自動待機メカニズムに依存する。ハードコードされた待機やデフォルトタイムアウトの増加は避ける。
- **明確さ**: 意図を明確に示す説明的なテストとステップタイトルを使用する。複雑なロジックや明らかでないインタラクションを説明する場合にのみコメントを追加する。


### テスト構造
- **インポート**: `import { test, expect } from '@playwright/test';`で始める。
- **構成**: 機能に関連するテストを`test.describe()`ブロックでグループ化する。
- **フック**: `describe`ブロック内のすべてのテストに共通のセットアップアクション（例：ページへのナビゲーション）に`beforeEach`を使用する。
- **タイトル**: `機能 - 特定のアクションまたはシナリオ`などの明確な命名規則に従う。


### ファイル構成
- **場所**: すべてのテストファイルを`tests/`ディレクトリに保存する。
- **命名**: `<機能またはページ>.spec.ts`の規則を使用する（例：`login.spec.ts`、`search.spec.ts`）。
- **スコープ**: 主要なアプリケーション機能またはページごとに1つのテストファイルを目指す。

### アサーションのベストプラクティス
- **UI構造**: コンポーネントのアクセシビリティツリー構造を検証するため`toMatchAriaSnapshot`を使用する。これにより包括的でアクセシブルなスナップショットが提供される。
- **要素数**: ロケーターで見つかった要素数をアサートするため`toHaveCount`を使用する。
- **テキストコンテンツ**: 完全一致には`toHaveText`を、部分一致には`toContainText`を使用する。
- **ナビゲーション**: アクション後のページURLを検証するため`toHaveURL`を使用する。


## テスト構造の例

```typescript
import { test, expect } from '@playwright/test';

test.describe('映画検索機能', () => {
  test.beforeEach(async ({ page }) => {
    // 各テスト前にアプリケーションに移動
    await page.goto('https://debs-obrien.github.io/playwright-movies-app');
  });

  test('タイトルで映画を検索', async ({ page }) => {
    await test.step('検索を有効化して実行', async () => {
      await page.getByRole('search').click();
      const searchInput = page.getByRole('textbox', { name: 'Search Input' });
      await searchInput.fill('Garfield');
      await searchInput.press('Enter');
    });

    await test.step('検索結果を検証', async () => {
      // 検索結果のアクセシビリティツリーを検証
      await expect(page.getByRole('main')).toMatchAriaSnapshot(`
        - main:
          - heading "Garfield" [level=1]
          - heading "search results" [level=2]
          - list "movies":
            - listitem "movie":
              - link "poster of The Garfield Movie The Garfield Movie rating":
                - /url: /playwright-movies-app/movie?id=tt5779228&page=1
                - img "poster of The Garfield Movie"
                - heading "The Garfield Movie" [level=2]
      `);
    });
  });
});
```

## テスト実行戦略

1. **初回実行**: `npx playwright test --project=chromium`でテストを実行
2. **失敗のデバッグ**: テストの失敗を分析し、根本原因を特定
3. **反復**: 必要に応じてロケーター、アサーション、またはテストロジックを改良
4. **検証**: テストが一貫して合格し、意図した機能をカバーすることを確認
5. **レポート**: テスト結果と発見された問題についてフィードバックを提供

## 品質チェックリスト

テストを最終化する前に以下を確認：
- [ ] すべてのロケーターがアクセシブルで具体的で、strict modeの違反を避ける
- [ ] テストが論理的にグループ化され、明確な構造に従っている
- [ ] アサーションが意味があり、ユーザーの期待を反映している
- [ ] テストが一貫した命名規則に従っている
- [ ] コードが適切にフォーマットされ、コメントが記述されている
