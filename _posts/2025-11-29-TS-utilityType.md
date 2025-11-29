---
layout: post
title: "TS UtilityType入門編"
date: 2025-11-28
categories: [TypeScript]  
permalink: /TS-utility/
---
 # UtilityTypeについての学習   

## ジェネリクス  `<T>`
ジェネリクスは、型をパラメータとして受け取り、その型を使用してコードを書くことができる  
一般的にはtypeの頭文字である「T」を用いることが多い  

## Partial`<T>`  
オブジェクト型のすべてのプロパティを省略可能にすることができます  
```
interface User {
  name: string
  age: number
}
type PartialUser = Partial<User>
```

## Required`<T>`  
オブジェクト型のすべてのプロパティを必須にすることができます  
ユーザー情報を扱う際に、名前と年齢の両方が必ず存在することを保証したい場合などに使います  
```
interface User {
  name?: string
  age?: number
}

type RequiredUser = Required<User>
```

## Pick<T, K>  
指定したプロパティのみを取り出すことができます。オブジェクトの一部のプロパティのみを持つ新しい型を作成する際に使われます。  

## Omit<T, K>  
Omit<Tの中から　-マイナスK>というイメージ     
オブジェクト T から、指定したキー K を“引く（取り除く）”型を作ります 
```
例: Person型からworkとincomeを取り除いた型を生成する
interface Person {
  name: string
  age: number
  work: string
  income: number
}

type Child = Omit<Person, 'work' | 'income'>

const mySon: Child = {
  name: 'taro',
  age: 10
}
``` 
## NonNullable<T>  
null と undefined を除く  

### NonNullable と Omit の違いを比較
| 目的    | NonNullable<T>   | Omit<T, K>                |
| ----- | ---------------- | ------------------------- |
| 何を消す？ | null / undefined | プロパティ（キー）                 |
| 対象    | ユニオン型            | オブジェクト型                   |
| 形の変化  | 値の種類が減る          | オブジェクトの項目が減る              |
| 使用例   | 値のチェック・戻り値の安全性   | API のレスポンス調整・UIで不要なデータの除去 |

```
複数のUtilityTypeを組み合わせて型を作ることができる  
例. blueプロパティを取得して、オプショナルを取り除いた型を取得
type Color = {
  red: number
  blue?: number
  green: number
}

type RequiredBlue = Required<Pick<Color, 'blue'>>
// type RequiredBlue = {
//   blue: number
// }

const color: RequiredBlue = {
  blue: 255
}
```
## ReturnType<typeof ・・・>  
関数の戻り値の型を取得することができます  
```
例:関数fullNameの戻り値の型を取得する
const fullName = (last: string, first: string): string => `${last} ${first}`

// fullName関数の戻り値の型
type Hoge = ReturnType<typeof fullName>

// 以下はエラーとなる
const foo: Hoge = 12345
// 以下はエラーとならない
const bar: Hoge = 'bar'
```

## Exclude<T, U>  
- “T から U を取り除いた型” を作るもの  
- Union型(A または B のどちらかになる型)でよく使われる  
```
つまり、
Exclude<T, U> = T - U（T から U を引く）  

例：基本的なユニオン型から特定の型を除外する
type T = "a" | "b" | "c";
type U = "b";

type NewType = Exclude<T, U>;
```

## Extract<T, U>  
T と U の共通部分だけ抽出する  
```
例１：
type T = number | boolean | (() => void) | (() => number);
type U = Function;

type OnlyFunction = Extract<T, U>;

結果：
(() => void) | (() => number)
これらはFunctionなので。

例２：オブジェクトの型を取り出す
type Person =
  | {
      type: 'student'
      name: string
      grade: number
    }
  | {
      type: 'teacher'
      age: number
      subject: string
    }

type Student = Extract<Person, { type: 'student' }>
// type Student = {
//   type: 'student'
//   name: string
//   grade: number
// }

const student: Student = {
  type: 'student',
  name: 'Taro',
  grade: 1
}　
```

## Readonly<T>  
オブジェクトのすべてのプロパティを読み取り専用にします  
読み取り専用のプロパティの値は変更することができなくなります  
  
## Parameters<typeof ・・・>  
関数の引数の型を取得することができます  
生成される型はタプル型(順番と型の両方が決まっている配列のこと)となります 
```
// 文字列型と数値型を引数にとる関数
const createMessage = (name: string, age: number) => {
  return `${name}さんは${age}歳です`
}

// createMessage関数から引数の型を取得した型
type Hoge = Parameters<typeof createMessage>
// type Hoge = [string, number]

// 以下はエラーとなる
const foo: Hoge = [20, 'taro']
// 以下はエラーとならない
const bar: Hoge = ['taro', 20]
```


