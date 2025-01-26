# Server Actions

```js
// app\050_actions\components\form.js
"use client";

import { createItem } from "@/actions/createItem";
export default function ArticleForm() {
  return <form action={createItem}>{/* フォームデータ */}</form>;
}
```

```js
// src\actions\createItem.js
"use server";

import { ENDPOINT } from "@/constants";

export async function createItem(formData) {
  const id = formData.get("id");
  const title = formData.get("title");

  if (id === "" || title === "") return { msg: "入力フィールドが空です。" };

  try {
    const res = await fetch(ENDPOINT, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ id, title }),
    });
    const data = await res.json();

    return data;
  } catch (e) {
    return { msg: "登録に失敗しました。" };
  }
}
```
