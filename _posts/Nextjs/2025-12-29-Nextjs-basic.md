---
layout: post
title: "Next.jsの基礎"
date: 2026-01-10 (update)
categories: [Next.js]  
permalink: /Nextjs-basic/
---
ここではNext.jsの基礎を学びます  

### importを素早く入力する方法  
<Header /> エラーが出ているコンポーネント名に  
Ctrl + .  
を押下すると選択肢が出てくるので    
`Add import from './Header'`を選択     
`import Header from './Header'`がファイル上部に表示される  

## SSG ISR SSRの違い  
| 略語      | 正式名称                                | 一言説明            |
| ------- | ----------------------------------- | --------------- |
| **SSG** | Static Site Generation              | ビルド時にHTMLを作る    |
| **SSR** | Server Side Rendering               | アクセス時にHTMLを作る   |
| **ISR** | **Incremental Static Regeneration** | 静的HTMLを「少しずつ」更新 |

| 項目          | SSG  | ISR    | SSR  |
| ----------- | ---- | ------ | ---- |
| HTML生成タイミング | ビルド時 | ビルド＋定期 | 毎回   |
| 表示速度        | 最速   | 速い     | やや遅い |
| サーバー負荷      | 最小   | 小      | 大    |
| 最新データ       | ×    | △      | ◎    |
| 個人別表示       | ×    | ×      | ◎    |

SSG = 先に作る（速い）  
SSR = 都度作る（柔軟）  
ISR = SSG + 定期更新  
Next.js App Router ではSSGがデフォルト  

使い分け例：  
| ページ内容    | 方式  |
| -------- | --- |
| トップページ   | SSG |
| ブログ      | ISR |
| 商品一覧     | ISR |
| マイページ    | SSR |
| 管理画面     | SSR |
| ログイン後ページ | SSR |


