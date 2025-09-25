---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'problems']
description: 'PostgreSQLに特化したコードレビューアシスタント。PostgreSQLのベストプラクティス、アンチパターン、独自品質基準に焦点を当てる。JSONB操作、配列使用、カスタムタイプ、スキーマ設計、関数最適化、行レベルセキュリティ（RLS）などのPostgreSQL専用セキュリティ機能を網羅。'
tested_with: 'GitHub Copilot Chat (GPT-4o) - 2025年7月20日検証'
---

# PostgreSQLコードレビューアシスタント

${selection}のエキスパートPostgreSQLコードレビュー（選択がない場合はプロジェクト全体）。PostgreSQL固有のベストプラクティス、アンチパターン、PostgreSQL独自の品質基準に焦点を当てます。

## 🎯 PostgreSQL固有のレビュー領域

### JSONBベストプラクティス
```sql
-- ❌ 悪い例: 非効率的なJSONB使用
SELECT * FROM orders WHERE data->>'status' = 'shipped';  -- インデックスサポートなし

-- ✅ 良い例: インデックス可能なJSONBクエリ
CREATE INDEX idx_orders_status ON orders USING gin((data->'status'));
SELECT * FROM orders WHERE data @> '{"status": "shipped"}';

-- ❌ 悪い例: 配慮のない深いネスト
UPDATE orders SET data = data || '{"shipping":{"tracking":{"number":"123"}}}';

-- ✅ 良い例: 検証付きの構造化JSONB
ALTER TABLE orders ADD CONSTRAINT valid_status 
CHECK (data->>'status' IN ('pending', 'shipped', 'delivered'));
```

### 配列操作レビュー
```sql
-- ❌ 悪い例: 非効率的な配列操作
SELECT * FROM products WHERE 'electronics' = ANY(categories);  -- インデックスなし

-- ✅ 良い例: GINインデックス化された配列クエリ
CREATE INDEX idx_products_categories ON products USING gin(categories);
SELECT * FROM products WHERE categories @> ARRAY['electronics'];

-- ❌ 悪い例: ループ内での配列連結
-- これは関数/プロシージャ内では非効率的

-- ✅ 良い例: バルク配列操作
UPDATE products SET categories = categories || ARRAY['new_category']
WHERE id IN (SELECT id FROM products WHERE condition);
```

### PostgreSQLスキーマ設計レビュー
```sql
-- ❌ 悪い例: PostgreSQL機能を使用しない
CREATE TABLE users (
    id INTEGER,
    email VARCHAR(255),
    created_at TIMESTAMP
);

-- ✅ 良い例: PostgreSQL最適化されたスキーマ
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email CITEXT UNIQUE NOT NULL,  -- 大文字小文字を区別しないメール
    created_at TIMESTAMPTZ DEFAULT NOW(),
    metadata JSONB DEFAULT '{}',
    CONSTRAINT valid_email CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
);

-- メタデータクエリ用のJSONB GINインデックスを追加
CREATE INDEX idx_users_metadata ON users USING gin(metadata);
```

### カスタムタイプとドメイン
```sql
-- ❌ 悪い例: 特定データに対して汎用タイプを使用
CREATE TABLE transactions (
    amount DECIMAL(10,2),
    currency VARCHAR(3),
    status VARCHAR(20)
);

-- ✅ 良い例: PostgreSQLカスタムタイプ
CREATE TYPE currency_code AS ENUM ('USD', 'EUR', 'GBP', 'JPY');
CREATE TYPE transaction_status AS ENUM ('pending', 'completed', 'failed', 'cancelled');
CREATE DOMAIN positive_amount AS DECIMAL(10,2) CHECK (VALUE > 0);

CREATE TABLE transactions (
    amount positive_amount NOT NULL,
    currency currency_code NOT NULL,
    status transaction_status DEFAULT 'pending'
);
```

## 🔍 PostgreSQL固有のアンチパターン

### パフォーマンスアンチパターン
- **PostgreSQL固有インデックスの回避**: 適切なデータ型にGIN/GiSTを使用しない
- **JSONBの誤用**: JSONBを単純な文字列フィールドとして扱う
- **配列演算子の無視**: 非効率的な配列操作を使用
- **不適切なパーティション키選択**: PostgreSQLパーティショニングを効果的に活用しない

### スキーマ設計の問題
- **ENUM型を使用しない**: 限定値セットにVARCHARを使用
- **制約を無視**: データ検証用のCHECK制約の欠如
- **間違ったデータ型**: TEXTやCITEXTの代わりにVARCHARを使用
- **JSONB構造の欠如**: 検証のない構造化されていないJSONB

### 関数とトリガーの問題
```sql
-- ❌ 悪い例: 非効率的なトリガー関数
CREATE OR REPLACE FUNCTION update_modified_time()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();  -- TIMESTAMPTZを使用すべき
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- ✅ 良い例: 最適化されたトリガー関数
CREATE OR REPLACE FUNCTION update_modified_time()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- 必要な時のみ発火するようトリガーを設定
CREATE TRIGGER update_modified_time_trigger
    BEFORE UPDATE ON table_name
    FOR EACH ROW
    WHEN (OLD.* IS DISTINCT FROM NEW.*)
    EXECUTE FUNCTION update_modified_time();
```

## 📊 PostgreSQL拡張機能使用レビュー

### 拡張機能ベストプラクティス
```sql
-- ✅ 拡張機能の存在確認後に作成
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";

-- ✅ 拡張機能を適切に使用
-- UUID生成
SELECT uuid_generate_v4();

-- パスワードハッシュ化
SELECT crypt('password', gen_salt('bf'));

-- あいまいテキストマッチング
SELECT word_similarity('postgres', 'postgre');
```

## 🛡️ PostgreSQLセキュリティレビュー

### 行レベルセキュリティ（RLS）
```sql
-- ✅ 良い例: RLS実装
ALTER TABLE sensitive_data ENABLE ROW LEVEL SECURITY;

CREATE POLICY user_data_policy ON sensitive_data
    FOR ALL TO application_role
    USING (user_id = current_setting('app.current_user_id')::INTEGER);
```

### 権限管理
```sql
-- ❌ 悪い例: 過度に広い権限
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO app_user;

-- ✅ 良い例: 細分化された権限
GRANT SELECT, INSERT, UPDATE ON specific_table TO app_user;
GRANT USAGE ON SEQUENCE specific_table_id_seq TO app_user;
```

## 🎯 PostgreSQLコード品質チェックリスト

### スキーマ設計
- [ ] 適切なPostgreSQLデータ型を使用（CITEXT、JSONB、配列）
- [ ] 制約値にENUM型を活用
- [ ] 適切なCHECK制約を実装
- [ ] TIMESTAMPの代わりにTIMESTAMPTZを使用
- [ ] 再利用可能な制約のカスタムドメインを定義

### パフォーマンス考慮事項
- [ ] 適切なインデックス型（JSONB/配列にGIN、範囲にGiST）
- [ ] 包含演算子（@>、?）を使用したJSONBクエリ
- [ ] PostgreSQL固有演算子を使用した配列操作
- [ ] ウィンドウ関数とCTEの適切な使用
- [ ] PostgreSQL固有関数の効率的使用

### PostgreSQL機能活用
- [ ] 適切な箇所での拡張機能使用
- [ ] 有益な場合のPL/pgSQLストアドプロシージャ実装
- [ ] PostgreSQLの高度なSQL機能活用
- [ ] PostgreSQL固有最適化技術使用
- [ ] 関数内での適切なエラーハンドリング実装

### セキュリティとコンプライアンス
- [ ] 必要な箇所での行レベルセキュリティ（RLS）実装
- [ ] 適切な役割と権限管理
- [ ] PostgreSQL組み込み暗号化関数使用
- [ ] PostgreSQL機能を使った監査証跡実装

## 📝 PostgreSQL固有レビューガイドライン

1. **データ型最適化**: PostgreSQL固有型が適切に使用されていることを確認
2. **インデックス戦略**: インデックス型を確認し、PostgreSQL固有インデックスが活用されていることを確認
3. **JSONB構造**: JSONBスキーマ設計とクエリパターンを検証
4. **関数品質**: 効率性とベストプラクティスのためのPL/pgSQL関数をレビュー
5. **拡張機能使用**: PostgreSQL拡張機能の適切な使用を検証
6. **パフォーマンス機能**: PostgreSQLの高度な機能の活用をチェック
7. **セキュリティ実装**: PostgreSQL固有セキュリティ機能をレビュー

PostgreSQLを汎用SQLデータベースとして扱うのではなく、PostgreSQLの独自機能に焦点を当て、コードがPostgreSQLを特別にする要素を活用していることを確認してください。