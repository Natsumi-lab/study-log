---
layout: post
title: "TypeScript 練習問題"
date: 2025-12-06
categories: [TypeScript]  
permalink: /TS-practice/
---
## TypeScriptのコードを書いていて疑問に思ったところの解説  

TypeScript で「既存の型に足す」方法  
&（交差型 / intersection type）  
User & { posts: Post[] }  

「何も含まない」というのは 省略可能（optional） という意味  
dueDate?: string  

----------------------------------------------------
### readonly isDone: boolean  
### isDone: Readonly<boolean>  
### は、全く別物  

readonly isDone: boolean
「オブジェクトのプロパティ isDone を再代入不可（読み取り専用）にする」 という意味  

isDone: Readonly<boolean>  
何の意味もありません  
Readonly<boolean> は boolean 型に対して「プロパティを readonly にする」という処理をしようとするが  
boolean は プリミティブ型でプロパティを持たない  
よって Readonly<boolean> = boolean のまま  

| 書き方                         | 意味                       | 効果                  |
| --------------------------- | ------------------------ | ------------------- |
| `readonly isDone: boolean`  | プロパティ isDone を読み取り専用にする  | ✔ 書き換え不可になる         |
| `isDone: Readonly<boolean>` | boolean に Readonly を適用する | ❌ 何も変わらず単なる boolean |
-------------------------------------------------------
```
const user = { id: 3, name: "bob"}
type UserKey = keyof typeof user
// "id" | "name"


型レベルで扱うには「値」ではなく「型」に変換する必要があり、typeof userを使う。  
typeof user
// => { id: number; name: string }

keyof は オブジェクトのキー名を union 型として取り出す演算子
keyof { id: number; name: string }
// => "id" | "name"
```
-------------------------------------------------------
```
const user = { id: 3, name: "bob"} as const
type UserKey = keyof typeof user
type UserValue = typeof user[UserKey]

↓　２・３行目の短縮形（意味は全く同じ）

const user = { id: 3, name: "bob"} as const
type UserValue = typeof user[keyof typeof user];
```
-------------------------------------------------------
```
const fruits = ["apple","orange", "lemon" ]
type FruitsType = _________
// FruitsType = "apple" | "orange" | "lemon" となるように変換してください

type FruitsType = typeof fruits[number];
// "apple" | "orange" | "lemon"
```
```
解説：[]箱として見る
fruits[0] → "apple"
fruits[1] → "orange"
fruits[2] → "lemon"

つまり
type FruitsTuple = ["apple", "orange", "lemon"];
と同じ意味

fruits[数字]だから
fruits[number]
```

```
配列の要素を Union 型にしたいとき
typeof 配列[number]
この配列に入っているどれかの値という意味
```
-----------------------------------------

## 網羅性チェック  
「union 型のすべての種類を処理できているかをコンパイル時に確認すること  

union の要素を追加したとき  
switch が書き漏れたとき  
TypeScript が コンパイルエラー を出すのでエラーに気づくことができます。  
```
定番の書き方
const assertNever = (x: never): never => {
  throw new Error("Unexpected value " + x);
};

switch (value) {
  case "A":
  case "B":
    break;
  default:
    return assertNever(value);
}
```
## noUnusedLocals（ノー・アンユーズド・ローカルズ）  
TypeScript の設定項目  

noUnusedLocals: true
定義しただけで使われていないローカル変数があるとエラーにする