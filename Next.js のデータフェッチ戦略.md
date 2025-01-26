# Next.js のデータフェッチ戦略

## 1. サーバーコンポーネントでのフェッチ

### 使用すべき場合

1. **初期表示のデータ取得**

   - SEO が重要な場合
   - 初期ローディングのパフォーマンスを最適化したい場合
   - キャッシュを活用したい場合

2. **機密データの取り扱い**

   - API キーなどの秘密情報を使用する場合
   - データベースに直接アクセスする場合

3. **大量のデータ処理**
   - クライアントに負荷をかけたくない場合
   - バンドルサイズを小さく保ちたい場合

### 使い方

```tsx
import { ENDPOINT } from "@/constants";
import ArticleList from "./../../components/articleList";

const SsrPage = async () => {
  // 1. デフォルトのフェッチ: default
  // レンダリング: Static Site Generation (SSG)
  // - ビルド時にデータをフェッチし、HTMLを生成
  // - 暗黙的にforce-cacheと同じ挙動
  const articlesSSG = await fetch(ENDPOINT).then((res) => res.json());

  // 2. 明示的なキャッシュ設定: force-cache
  // レンダリング: Static Site Generation (SSG)
  // - 1番目と同じ挙動
  // - force-cacheで明示的にSSGであることを示す
  const articlesSSGForceCache = await fetch(ENDPOINT, {
    cache: "force-cache",
  }).then((res) => res.json());

  // 3. キャッシュなしのフェッチ: no-store
  // レンダリング: Server-Side Rendering (SSR)
  // - リクエストごとにサーバーでデータをフェッチ
  // - 常に最新のデータを表示
  // - サーバーの負荷が高くなる可能性あり
  const articlesSSR = await fetch(ENDPOINT, {
    cache: "no-store",
  }).then((res) => res.json());

  // 4. ISR設定（現在の実装）: next: { revalidate: 10 }
  // レンダリング: Incremental Static Regeneration (ISR)
  // - 初回はSSGとして生成
  // - 10秒ごとにバックグラウンドで再検証
  // - キャッシュと最新性のバランスを取る
  // - ユーザーには常にキャッシュされたページを表示しつつ、定期的に更新
  const articlesISR = await fetch(ENDPOINT, {
    next: { revalidate: 10 },
  }).then((res) => res.json());

  return (
    <div>
      <ArticleList list={articlesSSR} basePath="/010_SSR" />
    </div>
  );
};

export default SsrPage;
```

## 2. クライアントコンポーネントでのフェッチ

### 使用すべき場合

1. **ユーザーインタラクション後のデータ取得**

   - 検索機能
   - フィルタリング
   - ページネーション

2. **リアルタイムデータの取得**

   - チャット機能
   - ライブ更新
   - ユーザー通知

3. **ユーザー固有のデータ**
   - ログイン後の個人データ
   - ユーザーの操作履歴

## まとめ

### 基本方針

- デフォルトではサーバーコンポーネントでのフェッチを選択
- 必要な場合のみクライアントコンポーネントでのフェッチを使用

### 考慮すべき点

- パフォーマンス要件
- SEO の必要性
- データの更新頻度
- ユーザーインタラクションの有無
- セキュリティ要件
