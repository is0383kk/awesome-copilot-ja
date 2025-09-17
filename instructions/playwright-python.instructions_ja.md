---
description: '公式ドキュメントに基づくPlaywright Python AIテスト生成指針。'
applyTo: '**'
---

# Playwright Pythonテスト生成指針

## テスト作成ガイドライン

### コード品質標準
- **ロケータ**: 復元性とアクセシビリティのため、ユーザー向けのロールベースロケータ（get_by_role、get_by_label、get_by_text）を優先する。
- **アサーション**: expect API経由で自動再試行するweb-firstアサーションを使用する（例：expect(page).to_have_title(...)）。特に要素の可視性変化をテストする場合でない限り、より具体的なアサーションがより信頼性が高いため、expect(locator).to_be_visible()を避ける。
- **タイムアウト**: Playwrightの組み込み自動待機メカニズムに依存する。ハードコードされた待機や増加したデフォルトタイムアウトを避ける。
- **明確性**: 意図を明確に示す説明的なテストタイトル（例：def test_navigation_link_works():）を使用する。「ボタンをクリック」のような単純な動作を説明するためではなく、複雑なロジックを説明するためだけにコメントを追加する。

### テスト構造
- **インポート**: すべてのテストファイルは`from playwright.sync_api import Page, expect`で始める。
- **フィクスチャ**: ブラウザページと対話するため、テスト関数の引数として`page: Page`フィクスチャを使用する。
- **セットアップ**: 各テスト関数の始めに`page.goto()`のようなナビゲーションステップを配置する。複数のテストで共有されるセットアップアクションには、標準のPytestフィクスチャを使用する。

### ファイル構成
- **場所**: 専用のtests/ディレクトリにテストファイルを保存するか、既存のプロジェクト構造に従う。
- **命名**: Pytestによって発見されるため、テストファイルは`test_<feature-or-page>.py`命名規則に従う必要がある。
- **範囲**: 主要なアプリケーション機能やページごとに1つのテストファイルを目指す。

## アサーションのベストプラクティス
- **要素数**: ロケータによって見つかった要素数をアサートするには`expect(locator).to_have_count()`を使用する。
- **テキスト内容**: 正確なテキスト一致には`expect(locator).to_have_text()`を使用し、部分一致には`expect(locator).to_contain_text()`を使用する。
- **ナビゲーション**: ページURLを確認するには`expect(page).to_have_url()`を使用する。
- **アサーションスタイル**: より信頼性の高いUIテストのため、`assert`よりも`expect`を優先する。

## 例

```python
import re
import pytest
from playwright.sync_api import Page, expect

@pytest.fixture(scope="function", autouse=True)
def before_each_after_each(page: Page):
    # 各テスト前に開始URLに移動する。
    page.goto("https://playwright.dev/")

def test_main_navigation(page: Page):
    expect(page).to_have_url("https://playwright.dev/")

def test_has_title(page: Page):
    # タイトルが部分文字列を「含む」ことを期待する。
    expect(page).to_have_title(re.compile("Playwright"))

def test_get_started_link(page: Page):
    page.get_by_role("link", name="Get started").click()
    
    # ページにInstallationという名前の見出しがあることを期待する。
    expect(page.get_by_role("heading", name="Installation")).to_be_visible()
```

## テスト実行戦略

1. **実行**: pytestコマンドを使用してターミナルからテストを実行する。
2. **失敗デバッグ**: テスト失敗を分析し根本原因を特定する
