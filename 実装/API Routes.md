# API Routes

https://nextjs.org/docs/app/api-reference/file-conventions/route

```js
import { ENDPOINT } from "@/constants";
import { NextResponse } from "next/server";

export async function GET() {
  const data = await fetch(ENDPOINT).then((res) => res.json());
  return NextResponse.json(data);
}

export async function POST(request) {
  const formData = await request.formData();
  const id = formData.get("id");
  const title = formData.get("title");

  if (id === "" || title === "") {
    return NextResponse.json(
      { msg: "入力フィールドが空です。" },
      { status: 500 }
    );
  }

  try {
    const res = await fetch(ENDPOINT, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ id, title }),
    });
    const data = await res.json();
    return NextResponse.json(data);
  } catch (e) {
    return NextResponse.json({ msg: "登録に失敗しました。" }, { status: 500 });
  }
}

export const dynamic = "force-static";
```
