---
layout: post
title: "Next.js"
date: 2026-01-09
categories: [Next.js]  
permalink: /App-Router/  
---
ここではApp Routerについて学習します。    

| 項目         | App Router           |
| ---------- | -------------------- |
| 使うフォルダ     | `app/`               |
| ページ定義      | `page.tsx`           |
| レイアウト      | `layout.tsx`         |
| デフォルト      | **Server Component** |
| Reactの最新機能 | フル対応                 |
| 現在の主流      | ✅                    |

### App Router の考え方  
「フォルダ = URL」  
「page.tsx = 画面」  
```
app/
 ├ page.tsx        → /
 ├ about/
 │   └ page.tsx    → /about
 └ blog/
     └ page.tsx    → /blog
```
👉 index.tsx はもう使わない  

### layout.tsx = 共通の枠  
app/
 ├ layout.tsx   ← 全ページ共通（ヘッダー・フッター）
 ├ page.tsx
 └ blog/
     ├ layout.tsx ← blog配下だけの共通枠
     └ page.tsx

### Server Component がデフォルト  
何も書かない → Server Component  
- サーバーで実行
- DBに直接アクセス可能
- APIキーを安全に扱える
- JSバンドルが小さい  

### Client Component は明示する  
```
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
Client が必要な例  
- useState
- useEffect
- onClick
- フォーム入力
👉 必要なところだけ Client  


### App Router が向いている理由（実務視点）  
- パフォーマンス
- サーバーで描画
- クライアントJS削減
- 初期表示が速い
- セキュリティ
- DBアクセスを隠せる
- APIキーを漏らさない
- 保守性
- フォルダを見るだけで構造が分かる
- 責務分離が自然