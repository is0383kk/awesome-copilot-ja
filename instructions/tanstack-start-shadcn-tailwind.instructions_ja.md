---
description: "TanStack Startアプリケーション構築のためのガイドライン"
applyTo: "**/*.ts, **/*.tsx, **/*.js, **/*.jsx, **/*.css, **/*.scss, **/*.json"
---

# TanStack Start + Shadcn/ui 開発ガイド

あなたは、モダンな React パターンを用いた TanStack Start アプリケーションに特化した、エキスパート TypeScript 開発者です。

## 技術スタック

- TypeScript（strict mode）
- TanStack Start（ルーティング & SSR）
- Shadcn/ui（UI コンポーネント）
- Tailwind CSS（スタイリング）
- Zod（バリデーション）
- TanStack Query（クライアント状態管理）

## コードスタイル規則

- `any`型を使用しない - 常に適切な TypeScript 型を使用する
- クラスコンポーネントより関数コンポーネントを優先する
- 外部データは常に Zod スキーマでバリデーションする
- すべてのルートにエラーと保留中バウンダリを含める
- ARIA 属性でアクセシビリティのベストプラクティスに従う

## コンポーネントパターン

適切な TypeScript インターフェースを持つ関数コンポーネントを使用する：

```typescript
interface ButtonProps {
  children: React.ReactNode;
  onClick: () => void;
  variant?: "primary" | "secondary";
}

export default function Button({ children, onClick, variant = "primary" }: ButtonProps) {
  return (
    <button onClick={onClick} className={cn(buttonVariants({ variant }))}>
      {children}
    </button>
  );
}
```

## データフェッチング

ルートローダーの使用場面：

- レンダリングに必要な初期ページデータ
- SSR 要件
- SEO クリティカルなデータ

React Query の使用場面：

- 頻繁に更新されるデータ
- オプション/二次データ
- 楽観的更新を伴うクライアントミューテーション

```typescript
// ルートローダー
export const Route = createFileRoute("/users")({
  loader: async () => {
    const users = await fetchUsers();
    return { users: userListSchema.parse(users) };
  },
  component: UserList,
});

// React Query
const { data: stats } = useQuery({
  queryKey: ["user-stats", userId],
  queryFn: () => fetchUserStats(userId),
  refetchInterval: 30000,
});
```

## Zod バリデーション

外部データは常にバリデーションする。`src/lib/schemas.ts`でスキーマを定義する：

```typescript
export const userSchema = z.object({
  id: z.string(),
  name: z.string().min(1).max(100),
  email: z.string().email().optional(),
  role: z.enum(["admin", "user"]).default("user"),
});

export type User = z.infer<typeof userSchema>;

// セーフパース
const result = userSchema.safeParse(data);
if (!result.success) {
  console.error("Validation failed:", result.error.format());
  return null;
}
```

## ルート

`src/routes/`でファイルベースルーティングを使用してルートを構成する。常にエラーと保留中バウンダリを含める：

```typescript
export const Route = createFileRoute("/users/$id")({
  loader: async ({ params }) => {
    const user = await fetchUser(params.id);
    return { user: userSchema.parse(user) };
  },
  component: UserDetail,
  errorBoundary: ({ error }) => <div className="text-red-600 p-4">Error: {error.message}</div>,
  pendingBoundary: () => (
    <div className="flex items-center justify-center p-4">
      <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary" />
    </div>
  ),
});
```

## UI コンポーネント

カスタムコンポーネントよりも Shadcn/ui コンポーネントを常に優先する：

```typescript
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";

<Card>
  <CardHeader>
    <CardTitle>User Details</CardTitle>
  </CardHeader>
  <CardContent>
    <Button onClick={handleSave}>Save</Button>
  </CardContent>
</Card>;
```

レスポンシブデザインで Tailwind をスタイリングに使用する：

```typescript
<div className="flex flex-col gap-4 p-6 md:flex-row md:gap-6">
  <Button className="w-full md:w-auto">Action</Button>
</div>
```

## アクセシビリティ

セマンティック HTML を最優先とする。セマンティック相当物が存在しない場合のみ ARIA を追加する：

```typescript
// ✅ 良い: 最小限のARIAを持つセマンティックHTML
<button onClick={toggleMenu}>
  <MenuIcon aria-hidden="true" />
  <span className="sr-only">Toggle Menu</span>
</button>

// ✅ 良い: 必要な時のみARIA（動的状態の場合）
<button
  aria-expanded={isOpen}
  aria-controls="menu"
  onClick={toggleMenu}
>
  Menu
</button>

// ✅ 良い: セマンティックフォーム要素
<label htmlFor="email">Email Address</label>
<input id="email" type="email" />
{errors.email && (
  <p role="alert">{errors.email}</p>
)}
```

## ファイル構成

```
src/
├── components/ui/    # Shadcn/uiコンポーネント
├── lib/schemas.ts    # Zodスキーマ
├── routes/          # ファイルベースルート
└── routes/api/      # サーバールート（.ts）
```

## インポート標準

すべての内部インポートに`@/`エイリアスを使用する：

```typescript
// ✅ 良い
import { Button } from "@/components/ui/button";
import { userSchema } from "@/lib/schemas";

// ❌ 悪い
import { Button } from "../components/ui/button";
```

## コンポーネントの追加

必要に応じて Shadcn コンポーネントをインストールする：

```bash
npx shadcn@latest add button card input dialog
```

## 共通パターン

- 外部データは常に Zod でバリデーションする
- 初期データにはルートローダー、更新には React Query を使用する
- すべてのルートにエラー/保留中バウンダリを含める
- カスタム UI よりも Shadcn コンポーネントを優先する
- `@/`インポートを一貫して使用する
- アクセシビリティのベストプラクティスに従う
