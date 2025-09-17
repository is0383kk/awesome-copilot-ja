---
description: 'VueJS 3開発標準とComposition API及びTypeScriptでのベストプラクティス'
applyTo: '**/*.vue, **/*.ts, **/*.js, **/*.scss'
---

# VueJS 3 開発指示書

Composition API、TypeScript、モダンベストプラクティスを使用した高品質VueJS 3アプリケーション構築のための指示書。

## プロジェクトコンテキスト
- Vue 3.xとComposition APIをデフォルトとして使用
- 型安全性のためのTypeScript
- `<script setup>`構文を使用した単一ファイルコンポーネント（`.vue`）
- モダンビルドツール（Vite推奨）
- アプリケーション状態管理のためのPinia
- 公式Vueスタイルガイドとベストプラクティス

## 開発標準

### アーキテクチャ
- Options APIよりもComposition API（`setup`関数とcomposables）を優先
- スケーラビリティのためにコンポーネントとcomposablesを機能またはドメインで整理
- UI重視コンポーネント（プレゼンテーション）とロジック重視コンポーネント（コンテナ）を分離
- 再利用可能なロジックを`composables/`ディレクトリのcomposable関数に抽出
- 明確に定義されたアクション、状態、ゲッターでドメイン別にストアモジュール（Pinia）を構造化

### TypeScript統合
- 最大型安全性のために`tsconfig.json`で`strict`モードを有効化
- `defineComponent`または`defineProps`と`defineEmits`を使った`<script setup lang="ts">`を使用
- 型付きpropsとデフォルト値に`PropType<T>`を活用
- 複雑なpropと状態の形にはインターフェースまたは型エイリアスを使用
- イベントハンドラー、refs、`useRoute`/`useRouter`フックの型を定義
- 適用可能な場所でジェネリックコンポーネントとcomposablesを実装

### コンポーネント設計
- コンポーネントに対して単一責任原則を遵守
- コンポーネント名にはPascalCase、ファイル名にはkebab-caseを使用
- コンポーネントを小さく、一つの関心事に焦点を当てる
- 簡潔性とパフォーマンスのために`<script setup>`構文を使用
- TypeScriptでpropsを検証；必要な場合のみランタイムチェックを使用
- 柔軟な構成のためにslotsとscoped slotsを優先

### 状態管理
- グローバル状態にはPiniaを使用：`defineStore`でストアを定義
- 単純なローカル状態には`setup`内で`ref`と`reactive`を使用
- 派生状態には`computed`を使用
- 複雑な構造のために状態を正規化
- 非同期ロジックにはPiniaストアのアクションを使用
- 永続化やデバッグのためにストアプラグインを活用

### Composition APIパターン
- 共有ロジック（例：`useFetch`、`useAuth`）のために再利用可能なcomposablesを作成
- 精密な依存関係リストで`watch`と`watchEffect`を使用
- `onUnmounted`または`watch`クリーンアップコールバックでサイドエフェクトをクリーンアップ
- 深い依存関係注入には`provide`/`inject`を控えめに使用
- `useAsyncData`またはサードパーティデータユーティリティ（Vue Query）を使用

### スタイリング
- コンポーネントレベルスタイルまたはCSSモジュールには`<style scoped>`を使用
- 高速スタイリングのためにユーティリティファーストフレームワーク（Tailwind CSS）を検討
- クラス命名にはBEMまたは機能的CSS規約に従う
- テーマとデザイントークンにはCSSカスタムプロパティを活用
- CSS GridとFlexboxでモバイルファースト、レスポンシブデザインを実装
- スタイルがアクセシブル（コントラスト、フォーカス状態）であることを確保

### パフォーマンス最適化
- 動的インポートと`defineAsyncComponent`でコンポーネントを遅延ロード
- 非同期コンポーネント読み込みフォールバックに`<Suspense>`を使用
- 静的または変更頻度の低い要素に`v-once`と`v-memo`を適用
- Vue DevToolsパフォーマンスタブでプロファイル
- 不要なウォッチャーを避ける；可能な場合は`computed`を優先
- 未使用コードをツリーシェイクし、Viteの最適化機能を活用

### データフェッチング
- `useFetch`（Nuxt）またはVue Queryのようなライブラリのcomposablesを使用
- ローディング、エラー、成功状態を明示的に処理
- コンポーネントアンマウントまたはパラメータ変更時に古いリクエストをキャンセル
- 失敗時のロールバックでオプティミスティック更新を実装
- レスポンスをキャッシュし、バックグラウンド再検証を使用

### エラーハンドリング
- キャッチされていないエラーにはグローバルエラーハンドラー（`app.config.errorHandler`）を使用
- リスキーなロジックを`try/catch`でラップ；ユーザーフレンドリーなメッセージを提供
- ローカル境界のためにコンポーネントで`errorCaptured`フックを使用
- フォールバックUIまたはエラーアラートを優雅に表示
- 外部サービス（Sentry、LogRocket）にエラーをログ記録

### フォームと検証
- 宣言的検証にはVeeValidateや@vueuse/formのようなライブラリを使用
- 制御された`v-model`バインディングでフォームを構築
- パフォーマンスのためにデバウンシングでblurまたはinput時に検証
- composablesでファイルアップロードと複雑な多段階フォームを処理
- アクセシブルなラベリング、エラー告知、フォーカス管理を確保

### ルーティング
- `createRouter`と`createWebHistory`でVue Router 4を使用
- ネストされたルートとルートレベルコード分割を実装
- ナビゲーションガード（`beforeEnter`、`beforeEach`）でルートを保護
- プログラム的ナビゲーションのために`setup`で`useRoute`と`useRouter`を使用
- クエリパラメータと動的セグメントを適切に管理
- ルートメタフィールドを通じてブレッドクラムデータを実装

### テスト
- Vue Test UtilsとJestで単体テストを記述
- 実装詳細ではなく動作に焦点を当てる
- コンポーネント分離のために`mount`と`shallowMount`を使用
- 必要に応じてグローバルプラグイン（router、Pinia）をモック
- CypressまたはPlaywrightでエンドツーエンドテストを追加
- axe-core統合を使用してアクセシビリティをテスト

### セキュリティ
- `v-html`の使用を避ける；HTMLインプットを厳格に無害化
- XSSとインジェクション攻撃を軽減するためにCSPヘッダーを使用
- テンプレートとディレクティブでデータを検証・エスケープ
- すべてのAPIリクエストにHTTPSを使用
- `localStorage`ではなくHTTP専用クッキーに機密トークンを保存

### アクセシビリティ
- セマンティックHTML要素とARIA属性を使用
- モーダルと動的コンテンツのフォーカス管理
- インタラクティブコンポーネントにキーボードナビゲーションを提供
- 画像とアイコンに意味のある`alt`テキストを追加
- カラーコントラストがWCAG AA標準を満たすことを確保

## 実装プロセス
1. コンポーネントとcomposableアーキテクチャを計画
2. Vue 3とTypeScriptでViteプロジェクトを初期化
3. Piniaストアとcomposablesを定義
4. コアUIコンポーネントとレイアウトを作成
5. ルーティングとナビゲーションを統合
6. データフェッチングと状態ロジックを実装
7. 検証とエラー状態を持つフォームを構築
8. グローバルエラーハンドリングとフォールバックUIを追加
9. 単体テストとE2Eテストを追加
10. パフォーマンスとバンドルサイズを最適化
11. アクセシビリティコンプライアンスを確保
12. コンポーネント、composables、ストアを文書化

## 追加ガイドライン
- Vueの公式スタイルガイド（vuejs.org/style-guide）に従う
- コード一貫性のためにESLint（`plugin:vue/vue3-recommended`と）とPrettierを使用
- 意味のあるコミットメッセージを書き、クリーンなgit履歴を維持
- 依存関係を最新に保ち、脆弱性を監査
- JSDoc/TSDocで複雑なロジックを文書化
- デバッグとプロファイリングにVue DevToolsを使用

## 共通パターン
- 柔軟なUIのためのレンダーレスコンポーネントとscoped slots
- provide/injectを使用したコンパウンドコンポーネント
- 横断的関心事のためのカスタムディレクティブ
- モーダルとオーバーレイのためのTeleport
- グローバルユーティリティ（i18n、アナリティクス）のためのプラグインシステム
- パラメータ化されたロジックのためのComposableファクトリー
