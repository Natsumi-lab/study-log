---
layout: post
title: "ReactによくでるJS"
date: 2025-12-12 (updated)
categories: [React]  
permalink: /react-start/
---
# Reactでよく使う const / function / 配列 / オブジェクトの基本まとめ

Reactでは主に **const** と **function** を使ってコードを書きます。  
**let はほとんど使用しません。**

---

## よく使う記法一覧

| 作りたいもの | 記号の組み合わせ | 例 |
|--------------|------------------|----|
| 関数コンポーネント | function App() {} | function App() {} |
| 関数コンポーネント（アロー関数） | const App = () => {} | const App = () => {} |
| 普通の変数 | const name = '太郎' | const name = "" |
| オブジェクト | const obj = {} | const user = {} |
| 配列 | const arr = [] | const skills = [] |
| useStateの受け取り | const [a, b] = useState() | const [count, setCount] = useState(0) |
| 関数（アロー関数） | const fn = () => {} | const handleClick = () => {} |
| オブジェクト分割代入 | const {a, b} = obj | const {name, age} = user |

---

## 関数コンポーネントの作り方

### function を使う方法

    function App() {
      return <h1>Hello</h1>
    }

### const を使う方法（アロー関数）

    const App = () => {
      return <h1>Hello</h1>
    }

形：

const + = + () => {}

---

## 変数の作り方

    const userName = '太郎'
    const number = 10
    const isLoggedIn = true

形：

const + 変数名 + = + 値

---

## オブジェクトの作り方

    const user = {
      name: '太郎',
      age: 20
    }

形：

const + 変数名 + = + {}

---

## 配列の作り方

    const skills = ['HTML', 'CSS', 'JavaScript']

形：

const + 変数名 + = + []

---

## useState の分割代入

    const [count, setCount] = useState(0)

useState は配列を返すため [] で受け取ります。

形：

const + [] + = + useState()

---

## アロー関数の定義

    const handleClick = () => {
      console.log('クリックされました')
    }

形：

const + 変数名 + = + () => {}

---

## オブジェクトの分割代入

    const { name, age } = user

形：

const + {} + = + オブジェクト

---

# Reactで let をほぼ使わない理由

Reactでは再レンダリング時の状態管理に **useState** を使います。

    const [count, setCount] = useState(0)

整理：

- 値が変わる → useState
- 値が変わらない → const

そのため let はほぼ使用しません。

---

# オブジェクトと配列の値変更

const で宣言しても **中身は変更可能** です。

例：

オブジェクトの値変更

    const fruit = {
      name: 'レモン',
      stock: 20
    }

    fruit.name = 'グレープフルーツ'
    fruit.color = 'pink'

---

配列の値変更

    const basket = ['いちご', 'さくらんぼ']

    basket[0] = 'マンゴー'
    basket.push('ライチ')

---

# 関数とは

関数は複数の処理をまとめたものです。

書き方：

function 関数名 (引数) { 処理 }

例：

    const double = count => count * 2

特徴：

- 引数が1つなら () を省略可能
- 処理が1行なら return を省略可能

---

# 分割代入

配列やオブジェクトから値を取り出す方法です。

例：

    const myFavoriteAnimal = ['ロッキー', 'dog']

    const [name, type] = myFavoriteAnimal

---

# スプレッド構文

記法：

...

配列やオブジェクトをコピー・追加できます。

配列コピー

    const numbers = [10, 20]
    const copyNumbers = [...numbers]

配列追加

    const updatedNumbers = [...numbers, 30, 40]

オブジェクトコピー

    const user = { id: 1, age: 20 }
    const copyUser = { ...user }

オブジェクト追加

    const updatedUser = {
      ...user,
      job: 'teacher'
    }

関数引数展開

    const array = [2, 5]

    const addFunction = (a, b) => {
      return console.log(a + b)
    }

    addFunction(...array)

---

# map（配列操作）

map は配列を加工して新しい配列を作ります。

例：

    const users = [
      { name: 'Williams' },
      { name: 'Brown' }
    ]

    users.map(user => 'Mr. ' + user.name)

---

# filter（配列操作）

条件に一致する要素だけ取り出します。

例：

    [1, 2, 3, 4, 5, '1'].filter(number => number !== '1')

結果：

    [1, 2, 3, 4, 5]

---

# 三項演算子

書き方：

条件 ? 真の場合 : 偽の場合

例：

    let age = 10

    let message = age >= 18
      ? '成人です'
      : '未成年です'

---

## null / undefined チェック例

    todos === null || todos === undefined
      ? undefined
      : todos.map(...)

意味：

todos が null または undefined の場合は処理しない  
それ以外なら map を実行する