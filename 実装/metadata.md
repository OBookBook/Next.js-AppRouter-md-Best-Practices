# 静的メタデータ

```js
// app\030_SG_fetch\page.js
export const metadata = {
  title: "フェッチ&SG",
  description: "フェッチしたデータで静的なページを作成しています。",
};
```

# 動的メタデータ

```js
// app\030_SG_fetch\[id]\page.js
export async function generateMetadata({ params }) {
  const article = await fetch(`${ENDPOINT}/${params.id}`).then((res) =>
    res.json()
  );

  return {
    title: article.title,
    description: article.description,
  };
}
```
