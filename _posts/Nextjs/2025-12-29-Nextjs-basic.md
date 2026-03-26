---
layout: post
title: "Next.jsの基礎"
date: 2026-01-10 (update)
categories: [Next.js]  
permalink: /Nextjs-basic/
---
# Next.js の基礎まとめ

ここでは Next.js の基礎を学びます。

---

## import を素早く入力する方法

コンポーネントにエラーが出ているとき：

例

    <Header />

この状態でコンポーネント名にカーソルを合わせて

Ctrl + .

を押すと候補が表示されます。

次を選択：

Add import from './Header'

するとファイル上部に自動で追加されます：

    import Header from './Header'

---

## SSG・ISR・SSR の違い

| 略語 | 正式名称 | 一言説明 |
|------|----------|----------|
| SSG | Static Site Generation | ビルド時にHTMLを作る |
| SSR | Server Side Rendering | アクセス時にHTMLを作る |
| ISR | Incremental Static Regeneration | 静的HTMLを少しずつ更新 |

---

## 生成タイミングの違い

| 項目 | SSG | ISR | SSR |
|------|-----|-----|-----|
| HTML生成タイミング | ビルド時 | ビルド＋定期更新 | 毎回アクセス時 |
| 表示速度 | 最速 | 速い | やや遅い |
| サーバー負荷 | 最小 | 小 | 大 |
| 最新データ | × | △ | ◎ |
| 個人別表示 | × | × | ◎ |

---

## 覚え方

SSG = 先に作る（速い）  
SSR = 都度作る（柔軟）  
ISR = SSG + 定期更新

Next.js App Router では **SSG がデフォルト** です。

---

## 使い分け例

| ページ内容 | 推奨方式 |
|------------|----------|
| トップページ | SSG |
| ブログ | ISR |
| 商品一覧 | ISR |
| マイページ | SSR |
| 管理画面 | SSR |
| ログイン後ページ | SSR |