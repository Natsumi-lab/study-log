---
layout: post
title: "INNER JOIN と LEFT JOIN の比較"
date: 2026-03-16
categories: [SQL]  
permalink: /InnerJoin-LeftJoin/
---
# INNER JOIN と LEFT JOIN の違い

SQLでは、複数のテーブルを組み合わせてデータを取得するために **JOIN** を使用します。  
その中でもよく使われるのが **INNER JOIN** と **LEFT JOIN** です。

---

# 1. INNER JOIN

## 概要

**両方のテーブルに存在するデータだけ取得する**

一致する行がある場合のみ結果に含まれます。

## 構文

```
SELECT カラム
FROM テーブルA
INNER JOIN テーブルB
ON 結合条件;
```

例  
users テーブル  
| id | name |
| -- | ---- |
| 1  | 佐藤   |
| 2  | 鈴木   |
| 3  | 田中   |

orders テーブル  
| id | user_id | product |
| -- | ------- | ------- |
| 1  | 1       | PC      |
| 2  | 2       | Mouse   |

```
SELECT users.name, orders.product
FROM users
INNER JOIN orders
ON users.id = orders.user_id;
```

結果  
| name | product |
| ---- | ------- |
| 佐藤   | PC      |
| 鈴木   | Mouse   |
※ ordersが存在しない「田中」は取得されない  

---

# 2. LEFT JOIN
左側のテーブルをすべて取得する  
右側のテーブルに一致するデータが無い場合はNULL が入る。  

```
構文
SELECT カラム
FROM テーブルA
LEFT JOIN テーブルB
ON 結合条件;

例
SELECT users.name, orders.product
FROM users
LEFT JOIN orders
ON users.id = orders.user_id;
```
結果  
| name | product |
| ---- | ------- |
| 佐藤   | PC      |
| 鈴木   | Mouse   |
| 田中   | NULL    |
※ ordersが無いユーザーも表示される  

---

# 3. INNER JOIN と LEFT JOIN の比較  

| JOIN       | 取得されるデータ          |
| ---------- | ----------------- |
| INNER JOIN | 両方のテーブルに存在するデータのみ |
| LEFT JOIN  | 左テーブルはすべて取得       |


users        orders
 -----        ------
 1 佐藤   ←→   user_id = 1
 2 鈴木   ←→   user_id = 2
 3 田中

INNER JOIN  
佐藤  
鈴木  

LEFT JOIN  
佐藤  
鈴木  
田中  

---

# 実務での使い分け  
### INNER JOIN  
「関連データが必ずある」場合  

例
- 注文したユーザー一覧  
- 投稿と投稿者

### LEFT JOIN  
「関連データが無い可能性がある」場合  

例  
- 全ユーザーと注文履歴
- 全記事とコメント

---

# まとめ  

| JOIN       | 特徴          |
| ---------- | ----------- |
| INNER JOIN | 共通データのみ取得   |
| LEFT JOIN  | 左テーブルをすべて取得 |

覚え方  
INNER = 共通部分  
LEFT = 左側は全部  

