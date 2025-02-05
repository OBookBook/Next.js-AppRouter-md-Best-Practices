```js
// next.config.mjs
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
  trailingSlash: true,
};

export default nextConfig;
```

```tsx:
// app\020_SG\[id]\page.js
export default async function Page({ params }) {
  return (
    <h3>
      このページは{params.id}です。{new Date().toJSON()}
    </h3>
  );
}

// 静的生成
export async function generateStaticParams() {
  return [{ id: "1" }, { id: "2" }];
}
// or
// データベースからパラメータを取得する
export async function generateStaticParams() {
  const data = await fetch(ENDPOINT).then((res) => res.json());
  return data.map((record) => ({ id: record.id }));
}

```

```shell
npm run build
```
