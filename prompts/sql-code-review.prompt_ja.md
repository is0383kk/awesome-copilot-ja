---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems']
description: 'すべての SQL データベース（MySQL、PostgreSQL、SQL Server、Oracle）における包括的なセキュリティ、保守性、コード品質分析を実行する汎用 SQL コードレビューアシスタント。SQL インジェクション防止、アクセス制御、コード標準、アンチパターン検出に焦点。完全な開発カバレッジのために SQL 最適化プロンプトを補完。'
tested_with: 'GitHub Copilot Chat (GPT-4o) - 2025年7月20日検証済み'
---

# SQL コードレビュー

${selection}（または選択がない場合はプロジェクト全体）の徹底的な SQL コードレビューを実行し、セキュリティ、パフォーマンス、保守性、データベースベストプラクティスに焦点を当てます。

## 🔒 セキュリティ分析

### SQL インジェクション防止
```sql
-- ❌ 重大: SQL インジェクション脆弱性
query = "SELECT * FROM users WHERE id = " + userInput;
query = f"DELETE FROM orders WHERE user_id = {user_id}";

-- ✅ 安全: パラメータ化クエリ
-- PostgreSQL/MySQL
PREPARE stmt FROM 'SELECT * FROM users WHERE id = ?';
EXECUTE stmt USING @user_id;

-- SQL Server
EXEC sp_executesql N'SELECT * FROM users WHERE id = @id', N'@id INT', @id = @user_id;
```

### アクセス制御・権限
- **最小権限の原則**: 必要最小限の権限を付与
- **ロールベースアクセス**: 直接的なユーザー権限ではなくデータベースロールを使用
- **スキーマセキュリティ**: 適切なスキーマ所有権とアクセス制御
- **関数/プロシージャセキュリティ**: DEFINER と INVOKER 権限のレビュー

### データ保護
- **機密データ露出**: 機密列を含むテーブルで SELECT * を避ける
- **監査ログ**: 機密操作がログに記録されることを確認
- **データマスキング**: ビューまたは関数を使用して機密データをマスク
- **暗号化**: 機密データの暗号化ストレージを確認

## ⚡ パフォーマンス最適化

### クエリ構造分析
```sql
-- ❌ 悪い: 非効率なクエリパターン
SELECT DISTINCT u.* 
FROM users u, orders o, products p
WHERE u.id = o.user_id 
AND o.product_id = p.id
AND YEAR(o.order_date) = 2024;

-- ✅ 良い: 最適化された構造
SELECT u.id, u.name, u.email
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.order_date >= '2024-01-01' 
AND o.order_date < '2025-01-01';
```

### インデックス戦略レビュー
- **欠落インデックス**: インデックスが必要な列を特定
- **過剰インデックス**: 未使用または冗長なインデックスを見つける
- **複合インデックス**: 複雑なクエリのための多列インデックス
- **インデックス保守**: 断片化または古いインデックスをチェック

### 結合最適化
- **結合タイプ**: 適切な結合タイプを確認（INNER vs LEFT vs EXISTS）
- **結合順序**: より小さな結果セットを最初に最適化
- **直積**: 欠落している結合条件を特定して修正
- **サブクエリ vs JOIN**: 最も効率的なアプローチを選択

### 集計・ウィンドウ関数
```sql
-- ❌ 悪い: 非効率な集計
SELECT user_id, 
       (SELECT COUNT(*) FROM orders o2 WHERE o2.user_id = o1.user_id) as order_count
FROM orders o1
GROUP BY user_id;

-- ✅ 良い: 効率的な集計
SELECT user_id, COUNT(*) as order_count
FROM orders
GROUP BY user_id;
```

## 🛠️ コード品質・保守性

### SQL スタイル・フォーマット
```sql
-- ❌ 悪い: 不適切なフォーマットとスタイル
select u.id,u.name,o.total from users u left join orders o on u.id=o.user_id where u.status='active' and o.order_date>='2024-01-01';

-- ✅ 良い: クリーンで読みやすいフォーマット
SELECT u.id,
       u.name,
       o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.status = 'active'
  AND o.order_date >= '2024-01-01';
```

### 命名規約
- **一貫した命名**: テーブル、列、制約が一貫したパターンに従う
- **説明的な名前**: データベースオブジェクトの明確で意味のある名前
- **予約語**: データベース予約語を識別子として使用することを避ける
- **大文字小文字の区別**: スキーマ全体で一貫した大文字小文字の使用

### スキーマ設計レビュー
- **正規化**: 適切な正規化レベル（過度/不足な正規化を避ける）
- **データ型**: ストレージとパフォーマンスに最適なデータ型選択
- **制約**: PRIMARY KEY、FOREIGN KEY、CHECK、NOT NULL の適切な使用
- **デフォルト値**: 列の適切なデフォルト値

## 🗄️ データベース固有ベストプラクティス

### PostgreSQL
```sql
-- JSON データに JSONB を使用
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    data JSONB NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- JSONB クエリ用 GIN インデックス
CREATE INDEX idx_events_data ON events USING gin(data);

-- 多値列に配列型を使用
CREATE TABLE tags (
    post_id INT,
    tag_names TEXT[]
);
```

### MySQL
```sql
-- 適切なストレージエンジンを使用
CREATE TABLE sessions (
    id VARCHAR(128) PRIMARY KEY,
    data TEXT,
    expires TIMESTAMP
) ENGINE=InnoDB;

-- InnoDB 用に最適化
ALTER TABLE large_table 
ADD INDEX idx_covering (status, created_at, id);
```

### SQL Server
```sql
-- 適切なデータ型を使用
CREATE TABLE products (
    id BIGINT IDENTITY(1,1) PRIMARY KEY,
    name NVARCHAR(255) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    created_at DATETIME2 DEFAULT GETUTCDATE()
);

-- 分析用 Columnstore インデックス
CREATE COLUMNSTORE INDEX idx_sales_cs ON sales;
```

### Oracle
```sql
-- 自動増分にシーケンスを使用
CREATE SEQUENCE user_id_seq START WITH 1 INCREMENT BY 1;

CREATE TABLE users (
    id NUMBER DEFAULT user_id_seq.NEXTVAL PRIMARY KEY,
    name VARCHAR2(255) NOT NULL
);
```

## 🧪 テスト・検証

### データ整合性チェック
```sql
-- 参照整合性を確認
SELECT o.user_id 
FROM orders o 
LEFT JOIN users u ON o.user_id = u.id 
WHERE u.id IS NULL;

-- データ一貫性をチェック
SELECT COUNT(*) as inconsistent_records
FROM products 
WHERE price < 0 OR stock_quantity < 0;
```

### パフォーマンステスト
- **実行プラン**: クエリ実行プランをレビュー
- **負荷テスト**: 現実的なデータ量でクエリをテスト
- **ストレステスト**: 並行負荷下でのパフォーマンスを確認
- **回帰テスト**: 最適化が機能を壊さないことを確認

## 📊 一般的なアンチパターン

### N+1 クエリ問題
```sql
-- ❌ 悪い: アプリケーションコードでの N+1 クエリ
for user in users:
    orders = query("SELECT * FROM orders WHERE user_id = ?", user.id)

-- ✅ 良い: 単一の最適化クエリ
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### DISTINCT の過度使用
```sql
-- ❌ 悪い: 結合問題をマスクする DISTINCT
SELECT DISTINCT u.name 
FROM users u, orders o 
WHERE u.id = o.user_id;

-- ✅ 良い: DISTINCT なしの適切な結合
SELECT u.name
FROM users u
INNER JOIN orders o ON u.id = o.user_id
GROUP BY u.name;
```

### WHERE 句での関数誤用
```sql
-- ❌ 悪い: 関数がインデックス使用を阻害
SELECT * FROM orders 
WHERE YEAR(order_date) = 2024;

-- ✅ 良い: 範囲条件がインデックスを使用
SELECT * FROM orders 
WHERE order_date >= '2024-01-01' 
  AND order_date < '2025-01-01';
```

## 📋 SQL レビューチェックリスト

### セキュリティ
- [ ] すべてのユーザー入力がパラメータ化されている
- [ ] 文字列連結による動的 SQL 構築がない
- [ ] 適切なアクセス制御と権限
- [ ] 機密データが適切に保護されている
- [ ] SQL インジェクション攻撃ベクターが排除されている

### パフォーマンス
- [ ] 頻繁にクエリされる列にインデックスが存在
- [ ] 不要な SELECT * ステートメントがない
- [ ] JOIN が最適化され適切なタイプを使用
- [ ] WHERE 句が選択的でインデックスを使用
- [ ] サブクエリが最適化または JOIN に変換されている

### コード品質
- [ ] 一貫した命名規約
- [ ] 適切なフォーマットとインデント
- [ ] 複雑なロジックの意味のあるコメント
- [ ] 適切なデータ型が使用されている
- [ ] エラー処理が実装されている

### スキーマ設計
- [ ] テーブルが適切に正規化されている
- [ ] 制約がデータ整合性を強制
- [ ] インデックスがクエリパターンをサポート
- [ ] 外部キー関係が定義されている
- [ ] デフォルト値が適切

## 🎯 レビュー出力形式

### 問題テンプレート
```
## [優先度] [カテゴリ]: [簡潔な説明]

**場所**: [テーブル/ビュー/プロシージャ名と該当する行番号]
**問題**: [問題の詳細説明]
**セキュリティリスク**: [該当する場合 - インジェクションリスク、データ露出など]
**パフォーマンス影響**: [クエリコスト、実行時間への影響]
**推奨事項**: [コード例付きの具体的な修正]

**修正前**:
```sql
-- 問題のある SQL
```

**修正後**:
```sql
-- 改善された SQL
```

**期待される改善**: [パフォーマンス向上、セキュリティ利益]
```

### 総合評価
- **セキュリティスコア**: [1-10] - SQL インジェクション保護、アクセス制御
- **パフォーマンススコア**: [1-10] - クエリ効率、インデックス使用
- **保守性スコア**: [1-10] - コード品質、ドキュメント
- **スキーマ品質スコア**: [1-10] - 設計パターン、正規化

### 上位 3 つの優先アクション
1. **[重要セキュリティ修正]**: SQL インジェクション脆弱性への対処
2. **[パフォーマンス最適化]**: 欠落インデックスの追加またはクエリ最適化
3. **[コード品質]**: 命名規約とドキュメントの改善

プラットフォーム固有の最適化とベストプラクティスを強調しながら、実用的でデータベース非依存の推奨事項の提供に焦点を当ててください。