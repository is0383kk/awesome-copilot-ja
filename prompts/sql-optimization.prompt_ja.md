---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems']
description: 'すべてのSQLデータベース（MySQL、PostgreSQL、SQL Server、Oracle）での包括的なクエリチューニング、インデックス戦略、データベースパフォーマンス解析を行うユニバーサルSQLパフォーマンス最適化アシスタント。実行プラン分析、ページネーション最適化、バッチ操作、パフォーマンス監視のガイダンスを提供します。'
tested_with: 'GitHub Copilot Chat (GPT-4o) - 2025年7月20日検証済み'
---

# SQLパフォーマンス最適化アシスタント

${selection}（または選択がない場合はプロジェクト全体）のエキスパートSQLパフォーマンス最適化。MySQL、PostgreSQL、SQL Server、Oracle、その他のSQLデータベースで動作するユニバーサルSQL最適化技術に重点を置きます。

## 🎯 主要最適化エリア

### クエリパフォーマンス解析
```sql
-- ❌ 悪い例：非効率なクエリパターン
SELECT * FROM orders o
WHERE YEAR(o.created_at) = 2024
  AND o.customer_id IN (
      SELECT c.id FROM customers c WHERE c.status = 'active'
  );

-- ✅ 良い例：適切なインデックスヒントを使った最適化クエリ
SELECT o.id, o.customer_id, o.total_amount, o.created_at
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
WHERE o.created_at >= '2024-01-01' 
  AND o.created_at < '2025-01-01'
  AND c.status = 'active';

-- 必要なインデックス:
-- CREATE INDEX idx_orders_created_at ON orders(created_at);
-- CREATE INDEX idx_customers_status ON customers(status);
-- CREATE INDEX idx_orders_customer_id ON orders(customer_id);
```

### インデックス戦略の最適化
```sql
-- ❌ 悪い例：不適切なインデックス戦略
CREATE INDEX idx_user_data ON users(email, first_name, last_name, created_at);

-- ✅ 良い例：最適化された複合インデックス
-- emailで先にフィルタし、created_atでソートするクエリ用
CREATE INDEX idx_users_email_created ON users(email, created_at);

-- 氏名のフルテキスト検索用
CREATE INDEX idx_users_name ON users(last_name, first_name);

-- ユーザーステータス検索用
CREATE INDEX idx_users_status_created ON users(status, created_at)
WHERE status IS NOT NULL;
```

### サブクエリ最適化
```sql
-- ❌ 悪い例：相関サブクエリ
SELECT p.product_name, p.price
FROM products p
WHERE p.price > (
    SELECT AVG(price) 
    FROM products p2 
    WHERE p2.category_id = p.category_id
);

-- ✅ 良い例：ウィンドウ関数のアプローチ
SELECT product_name, price
FROM (
    SELECT product_name, price,
           AVG(price) OVER (PARTITION BY category_id) as avg_category_price
    FROM products
) ranked
WHERE price > avg_category_price;
```

## 📊 パフォーマンス・チューニング技術

### JOIN最適化
```sql
-- ❌ 悪い例：非効率なJOIN順序と条件
SELECT o.*, c.name, p.product_name
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.id
LEFT JOIN order_items oi ON o.id = oi.order_id
LEFT JOIN products p ON oi.product_id = p.id
WHERE o.created_at > '2024-01-01'
  AND c.status = 'active';

-- ✅ 良い例：フィルタリングを使った最適化JOIN
SELECT o.id, o.total_amount, c.name, p.product_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id AND c.status = 'active'
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id
WHERE o.created_at > '2024-01-01';
```

### ページネーション最適化
```sql
-- ❌ 悪い例：OFFSETベースのページネーション（大きなオフセットで遅い）
SELECT * FROM products 
ORDER BY created_at DESC 
LIMIT 20 OFFSET 10000;

-- ✅ 良い例：カーソルベースのページネーション
SELECT * FROM products 
WHERE created_at < '2024-06-15 10:30:00'
ORDER BY created_at DESC 
LIMIT 20;

-- またはIDベースのカーソル使用
SELECT * FROM products 
WHERE id > 1000
ORDER BY id 
LIMIT 20;
```

### 集計最適化
```sql
-- ❌ 悪い例：複数の個別集計クエリ
SELECT COUNT(*) FROM orders WHERE status = 'pending';
SELECT COUNT(*) FROM orders WHERE status = 'shipped';
SELECT COUNT(*) FROM orders WHERE status = 'delivered';

-- ✅ 良い例：条件付き集計を使った単一クエリ
SELECT 
    COUNT(CASE WHEN status = 'pending' THEN 1 END) as pending_count,
    COUNT(CASE WHEN status = 'shipped' THEN 1 END) as shipped_count,
    COUNT(CASE WHEN status = 'delivered' THEN 1 END) as delivered_count
FROM orders;
```

## 🔍 クエリのアンチパターン

### SELECTのパフォーマンス問題
```sql
-- ❌ 悪い例：SELECT *のアンチパターン
SELECT * FROM large_table lt
JOIN another_table at ON lt.id = at.ref_id;

-- ✅ 良い例：明示的なカラム選択
SELECT lt.id, lt.name, at.value
FROM large_table lt
JOIN another_table at ON lt.id = at.ref_id;
```

### WHERE句の最適化
```sql
-- ❌ 悪い例：WHERE句での関数呼び出し
SELECT * FROM orders 
WHERE UPPER(customer_email) = 'JOHN@EXAMPLE.COM';

-- ✅ 良い例：インデックスフレンドリーなWHERE句
SELECT * FROM orders 
WHERE customer_email = 'john@example.com';
-- 検討事項: CREATE INDEX idx_orders_email ON orders(LOWER(customer_email));
```

### OR vs UNION最適化
```sql
-- ❌ 悪い例：複雑なOR条件
SELECT * FROM products 
WHERE (category = 'electronics' AND price < 1000)
   OR (category = 'books' AND price < 50);

-- ✅ 良い例：より良い最適化のためのUNIONアプローチ
SELECT * FROM products WHERE category = 'electronics' AND price < 1000
UNION ALL
SELECT * FROM products WHERE category = 'books' AND price < 50;
```

## 📈 データベース非依存最適化

### バッチ操作
```sql
-- ❌ 悪い例：行ごとの操作
INSERT INTO products (name, price) VALUES ('Product 1', 10.00);
INSERT INTO products (name, price) VALUES ('Product 2', 15.00);
INSERT INTO products (name, price) VALUES ('Product 3', 20.00);

-- ✅ 良い例：バッチ挿入
INSERT INTO products (name, price) VALUES 
('Product 1', 10.00),
('Product 2', 15.00),
('Product 3', 20.00);
```

### 一時テーブルの使用
```sql
-- ✅ 良い例：複雑な操作での一時テーブル使用
CREATE TEMPORARY TABLE temp_calculations AS
SELECT customer_id, 
       SUM(total_amount) as total_spent,
       COUNT(*) as order_count
FROM orders 
WHERE created_at >= '2024-01-01'
GROUP BY customer_id;

-- さらなる計算での一時テーブル使用
SELECT c.name, tc.total_spent, tc.order_count
FROM temp_calculations tc
JOIN customers c ON tc.customer_id = c.id
WHERE tc.total_spent > 1000;
```

## 🛠️ インデックス管理

### インデックス設計の原則
```sql
-- ✅ 良い例：カバリングインデックス設計
CREATE INDEX idx_orders_covering 
ON orders(customer_id, created_at) 
INCLUDE (total_amount, status);  -- SQL Server構文
-- または: CREATE INDEX idx_orders_covering ON orders(customer_id, created_at, total_amount, status); -- その他のデータベース
```

### 部分インデックス戦略
```sql
-- ✅ 良い例：特定条件の部分インデックス
CREATE INDEX idx_orders_active 
ON orders(created_at) 
WHERE status IN ('pending', 'processing');
```

## 📊 パフォーマンス監視クエリ

### クエリパフォーマンス解析
```sql
-- 遅いクエリを特定する汎用アプローチ
-- （具体的な構文はデータベースによって異なる）

-- MySQL用:
SELECT query_time, lock_time, rows_sent, rows_examined, sql_text
FROM mysql.slow_log
ORDER BY query_time DESC;

-- PostgreSQL用:
SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY total_time DESC;

-- SQL Server用:
SELECT 
    qs.total_elapsed_time/qs.execution_count as avg_elapsed_time,
    qs.execution_count,
    SUBSTRING(qt.text, (qs.statement_start_offset/2)+1,
        ((CASE qs.statement_end_offset WHEN -1 THEN DATALENGTH(qt.text)
        ELSE qs.statement_end_offset END - qs.statement_start_offset)/2)+1) as query_text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY avg_elapsed_time DESC;
```

## 🎯 ユニバーサル最適化チェックリスト

### クエリ構造
- [ ] プロダクションクエリでのSELECT *の回避
- [ ] 適切なJOINタイプの使用（INNER vs LEFT/RIGHT）
- [ ] WHERE句での早期フィルタリング
- [ ] 適切な場合のサブクエリでのINの代わりにEXISTSの使用
- [ ] インデックス使用を妨げるWHERE句での関数回避

### インデックス戦略
- [ ] 頻繁にクエリされるカラムでのインデックス作成
- [ ] 正しいカラム順序での複合インデックス使用
- [ ] 過度なインデックス化の回避（INSERT/UPDATEパフォーマンスに影響）
- [ ] 有益な場合のカバリングインデックス使用
- [ ] 特定のクエリパターンでの部分インデックス作成

### データ型とスキーマ
- [ ] ストレージ効率のための適切なデータ型使用
- [ ] 適切な正規化（OLTPには3NF、OLAPには非正規化）
- [ ] クエリオプティマイザーを助ける制約の使用
- [ ] 適切な場合の大きなテーブルのパーティション

### クエリパターン
- [ ] 結果セット制御でのLIMIT/TOP使用
- [ ] 効率的なページネーション戦略の実装
- [ ] 一括データ変更でのバッチ操作使用
- [ ] N+1クエリ問題の回避
- [ ] 繰り返しクエリでのプリペアドステートメント使用

### パフォーマンステスト
- [ ] 現実的なデータボリュームでのクエリテスト
- [ ] クエリ実行プランの分析
- [ ] 時間経過でのクエリパフォーマンス監視
- [ ] 遅いクエリでのアラート設定
- [ ] 定期的なインデックス使用状況分析

## 📝 最適化の方法論

1. **特定**: データベース固有のツールを使用して遅いクエリを見つける
2. **分析**: 実行プランを調査してボトルネックを特定する
3. **最適化**: 適切な最適化技術を適用する
4. **テスト**: パフォーマンス改善を検証する
5. **監視**: パフォーマンスメトリクスを継続的に追跡する
6. **反復**: 定期的なパフォーマンスレビューと最適化

測定可能なパフォーマンス改善に焦点を当て、現実的なデータボリュームとクエリパターンで常に最適化をテストしてください。