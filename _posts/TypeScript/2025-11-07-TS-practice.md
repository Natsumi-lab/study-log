---
layout: post
title: "TypeScript 練習問題"
date: 2025-12-06
categories: [TypeScript]  
permalink: /TS-practice/
---
# TypeScript のコードで疑問に思いやすいポイントまとめ

ここでは TypeScript を書いていて混乱しやすい型操作について整理します。

---

## 既存の型に型を追加する方法（交差型）

記法：

User & { posts: Post[] }

意味：

既存の User 型に posts プロパティを追加した新しい型を作る

これは **intersection type（交差型）** と呼ばれます。

---

## optional（省略可能プロパティ）

記法：

dueDate?: string

意味：

「プロパティが存在しなくてもよい」

例：

- dueDate がある場合 → string
- dueDate がない場合 → OK

---

# readonly の違い

次の2つは見た目が似ていますが意味が違います。

---

## readonly isDone: boolean

意味：

オブジェクトのプロパティ isDone を再代入不可にする

例：

    type Todo = {
      readonly isDone: boolean
    }

これは書き換え不可になります。

---

## isDone: Readonly<boolean>

意味：

実質的に意味がありません

理由：

Readonly はオブジェクト型に対して使うユーティリティ型ですが  
boolean はプリミティブ型なので変化しません

結果：

boolean のまま

---

## 比較まとめ

| 書き方 | 意味 | 効果 |
|--------|------|------|
| readonly isDone: boolean | プロパティを読み取り専用にする | 書き換え不可 |
| isDone: Readonly<boolean> | boolean に Readonly を適用 | 変化なし |

---

# keyof と typeof の組み合わせ

例：

    const user = { id: 3, name: "bob" }

    type UserKey = keyof typeof user

結果：

"id" | "name"

---

## 仕組み

typeof user

意味：

値を型として取得

結果：

{ id: number; name: string }

---

keyof

意味：

オブジェクトのキー名を union 型として取得

結果：

"id" | "name"

---

# オブジェクトの value を union 型にする

例：

    const user = { id: 3, name: "bob" } as const

    type UserValue = typeof user[keyof typeof user]

結果：

3 | "bob"

---

# 配列の要素を union 型にする方法

例：

    const fruits = ["apple", "orange", "lemon"]

    type FruitsType = typeof fruits[number]

結果：

"apple" | "orange" | "lemon"

---

## 考え方

配列は箱として考えます

例：

fruits[0] → "apple"  
fruits[1] → "orange"  
fruits[2] → "lemon"

つまり：

fruits[number]

は

配列のどれかの値

という意味になります。

---

## まとめ（配列 → union 型）

記法：

typeof 配列[number]

意味：

配列に含まれる値のどれか

---

# 網羅性チェック（exhaustiveness check）

union 型のすべてのケースを処理できているかを  
コンパイル時にチェックする方法です。

新しい union を追加したとき

switch 文に書き漏れがあると  
TypeScript がエラーを出します。

---

## 定番の書き方

    const assertNever = (x: never): never => {
      throw new Error("Unexpected value " + x)
    }

    switch (value) {
      case "A":
      case "B":
        break
      default:
        return assertNever(value)
    }

---

# noUnusedLocals

TypeScript の設定項目です

設定：

    noUnusedLocals: true

意味：

宣言しただけで使われていないローカル変数がある場合  
コンパイルエラーになります

目的：

不要な変数を検出してコード品質を高める