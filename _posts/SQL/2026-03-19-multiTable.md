---
layout: post
title: "複数の仮想テーブル"
date: 2026-03-19
categories: [SQL]  
permalink: /Multi-Derived-Table/
---
# 仮想テーブルを複数作成する方法

SQLでは、仮想テーブル（Derived Table）を複数作成して組み合わせることで、複雑なデータ処理を分かりやすく整理できます。

---

# 1. 方法①：FROM句に複数の仮想テーブルを書く

## 概要
複数のサブクエリをそれぞれ仮想テーブルとして定義し、JOINで結合する。

## 例

    SELECT a.user_id, a.total, b.avg_amount
    FROM (
      SELECT user_id, SUM(amount) AS total
      FROM orders
      GROUP BY user_id
    ) a
    JOIN (
      SELECT AVG(amount) AS avg_amount
      FROM orders
    ) b
    ON true;

## ポイント
- 仮想テーブルごとに別名が必要
- JOINで組み合わせる
- 小規模な処理に向いている

---

# 2. 方法②：CTE（WITH句）を使う

## 概要
複数の仮想テーブルを名前付きで定義できる方法  
可読性が高く、実務でよく使われる

## 例

    WITH total_per_user AS (
      SELECT user_id, SUM(amount) AS total
      FROM orders
      GROUP BY user_id
    ),
    avg_amount AS (
      SELECT AVG(amount) AS avg_amount
      FROM orders
    )
    SELECT t.user_id, t.total, a.avg_amount
    FROM total_per_user t
    JOIN avg_amount a
    ON true;

## ポイント
- 複数の仮想テーブルを並べて定義できる
- 名前を付けて再利用できる
- 読みやすくなる

---

# 3. どちらを使うべきか

| 方法 | 特徴 |
|------|------|
| FROMに直接書く | シンプルだが読みにくくなりやすい |
| WITH（CTE） | 可読性が高く、複雑な処理に向いている |

---

# 4. 使い分けの目安

## FROM句の仮想テーブル
- 簡単な集計
- 一度しか使わない処理

## CTE（WITH句）
- 複雑なロジック
- 複数の仮想テーブルを使う
- チーム開発や保守性重視

---

# まとめ

- 仮想テーブルは複数作成できる
- JOINで組み合わせるのが基本
- 複雑な場合はWITH句（CTE）を使うと整理しやすい