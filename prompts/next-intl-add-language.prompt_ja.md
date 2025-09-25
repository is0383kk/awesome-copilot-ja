---
mode: 'agent'
tools: ['changes','codebase', 'editFiles', 'findTestFiles', 'search', 'writeTest']
description: 'Next.js + next-intlアプリケーションに新しい言語を追加します'
---

これは国際化にnext-intlを使用するNext.jsプロジェクトに新しい言語を追加するガイドです。

- i18nについて、アプリケーションはnext-intlを使用します。
- すべての翻訳は`./messages`ディレクトリにあります。
- UIコンポーネントは`src/components/language-toggle.tsx`です。
- ルーティングとミドルウェア設定は以下で処理されます：
  - `src/i18n/routing.ts`
  - `src/middleware.ts`

新しい言語を追加する場合：

- `en.json`のすべてのコンテンツを新しい言語に翻訳します。目標は完全な翻訳のために、新しい言語ですべてのJSONエントリを持つことです。
- `routing.ts`と`middleware.ts`にパスを追加します。
- `language-toggle.tsx`に言語を追加します。