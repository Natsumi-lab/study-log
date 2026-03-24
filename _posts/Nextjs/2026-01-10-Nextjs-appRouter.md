---
layout: post
title: "Next.jsのApp Router"
date: 2026-03-24 (updated)
categories: [Next.js]  
permalink: /Nextjs-AppRouter/
---
# Next.js の App Router まとめ

## App Router とは

App Router は、Next.js の新しいルーティング方式です。  
`app` ディレクトリを基準にした **ファイルシステムベースのルーティング** を採用していて、Server Components を前提に設計されています。

レイアウト・ローディング表示・エラーハンドリング・API処理などを標準機能として扱えるのが特徴です。

---

## Pages Router との違い

Next.js には 2 つのルーターがあります。

| ルーター | 特徴 |
|----------|------|
| App Router | 新しい方式（推奨） |
| Pages Router | 従来方式（現在も使用可能） |

App Router は React の最新機能を前提とした設計になっています。

---

## 基本の考え方

App Router では、`app` フォルダ構成がそのまま URL 構造になります。

代表的なファイル：

| ファイル | 役割 |
|----------|------|
| page.tsx | ページ本体 |
| layout.tsx | 共通レイアウト |
| loading.tsx | 読み込み中UI |
| error.tsx | エラーUI |
| route.ts | API処理 |

---

## page.tsx の役割

`page.tsx` はそのURLの画面を表します。

例：

app/about/page.tsx  
→ /about に対応

フォルダ構造とURLが一致するため理解しやすい設計です。

---

## layout.tsx の役割

`layout.tsx` は共通UIを定義します。

例：

- ヘッダー
- サイドバー
- フッター

また App Router では **レイアウトをネストできます**

例：

全体レイアウト  
＋  
管理画面専用レイアウト

のように重ねて構築できます。

---

## Server Component がデフォルト

App Router ではページは標準で Server Component です。

特徴：

- サーバーでデータ取得できる
- 軽量なJavaScriptになる
- SEOに強い
- ストリーミング表示できる

---

## Client Component の使い方

Client Component はファイル先頭に次を書くことで指定します。

use client

使用場面：

- useState
- useEffect
- クリックイベント
- フォーム操作
- モーダル制御
- ブラウザAPI使用

考え方：

表示中心 → Server Component  
操作中心 → Client Component

---

## loading.tsx

読み込み中のUIを定義できます。

例：

- スケルトン表示
- ローディングスピナー

React Suspense と連携して段階表示（ストリーミング）が可能になります。

---

## error.tsx

ルート単位でエラーUIを表示できます。

特徴：

- ページ単位で制御可能
- アプリ全体を止めない
- 問題箇所だけ表示できる

---

## route.ts

APIエンドポイントを作成できます。

例：

GET / POST / PUT / DELETE などの処理を書けます。

従来の Pages Router の API Routes に相当します。

---

## App Router のメリット

| メリット | 内容 |
|----------|------|
| レイアウト管理が簡単 | ネスト構造で整理できる |
| Server Component対応 | データ取得と描画を統合できる |
| 状態別UI管理が容易 | loading.tsx / error.tsx |
| URL構造が分かりやすい | フォルダ構造と一致 |
| APIも同じ場所に書ける | route.ts |

---

## 学習のおすすめ順序

次の順で理解すると効率的です。

1. appフォルダとpage.tsx
2. layout.tsx
3. Server / Client Component
4. loading.tsx
5. error.tsx
6. route.ts

---

## まとめ

- App Router は Next.js の新しいルーティング方式
- app ディレクトリを基準にURLが決まる
- page.tsx はページ本体
- layout.tsx は共通レイアウト
- Server Component がデフォルト
- use client で Client Component 化
- loading.tsx と error.tsx で状態別UI管理
- route.ts でAPI処理が書ける  

