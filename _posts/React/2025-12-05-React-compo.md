---
layout: post
title: "React component"
date: 2025-12-05
categories: [React]  
permalink: /React-compo/
---

# React の props（プロップス）まとめ

## props とは

props は **親コンポーネントから子コンポーネントに渡すデータ** です。

子コンポーネントでは次のように受け取って使います：

    props.名前

---

## props の基本ルール

通常の props（数値・文字列など）は

親 → 子

の **一方向のみ** です。

ただし **関数を渡す場合だけ**

子 → 親

のやり取りが可能になります。

これは React で「子から親へ通知する唯一の方法」です。

---

## 子から親へ関数を渡す例

子コンポーネント：

    function MyComponent(props) {
      return <button onClick={props.onClick}>Click me</button>
    }

親コンポーネント：

    function App() {
      const handleClick = () => {
        alert("Button was clicked!")
      }

      return <MyComponent onClick={handleClick} />
    }

流れ：

App（親）

↓ 関数を渡す

onClick={handleClick}

↓

MyComponent（子）

↓

props.onClick() 実行

↓

親の handleClick() が呼ばれる

---

# props の分割代入

props から必要な値だけ取り出して使う方法です。

---

## 通常の書き方

    function MyComponent(props) {
      return <p>{props.message}</p>
    }

毎回 props.message と書く必要があります。

---

## 分割代入の書き方

    function MyComponent({ message }) {
      return <p>{message}</p>
    }

意味：

    const message = props.message

と同じです。

---

## 分割代入の比較

| 方法 | 説明 |
|------|------|
| props.message | 従来の書き方 |
| { message } | props から message を取り出す |
| { message, onClick } | 複数の props を取り出す |

---

# children とは

children は

props.children

の省略形です。

コンポーネントを「枠」として使えるようになります。

---

## children の例

親コンポーネント：

    function ParentComponent() {
      return (
        <ChildComponent>
          <p>この部分が props.children として渡されます</p>
        </ChildComponent>
      )
    }

子コンポーネント：

    function ChildComponent(props) {
      return (
        <>
          {props.children}
        </>
      )
    }

意味：

ChildComponent タグの中身が props.children に入る

---

## 分割代入と children の組み合わせ

    function ChildComponent({ children }) {
      return <div>{children}</div>
    }

---

# children を使うメリット

コンポーネントを「再利用できる枠」として使えます。

例：

レイアウトコンポーネント

カードコンポーネント

モーダルコンポーネント

などに便利です。

---

# デフォルト値の設定（論理 OR）

例：

    function Title({ text }) {
      return <h1>{text || "タイトルなし"}</h1>
    }

結果：

    <Title text="こんにちは" />

表示：

こんにちは

---

    <Title />

表示：

タイトルなし

---

# A || B の意味（JavaScript）

次のルールで判定されます：

A が

- 空
- undefined
- null
- false

なら

B を採用

A に値があれば

A を採用

---

# 親コンポーネントと子コンポーネント

## 親コンポーネント

他のコンポーネントを含むコンポーネント

---

## 子コンポーネント

親の中に書かれているコンポーネント

---

## 例：データを渡す

親：

    const ParentComponent = () => {
      const user = {
        name: "太郎",
        age: 25
      }

      return (
        <ChildComponent
          name={user.name}
          age={user.age}
        />
      )
    }

子：

    const ChildComponent = (props) => {
      return (
        <div>
          <p>名前: {props.name}</p>
          <p>年齢: {props.age}</p>
        </div>
      )
    }

---

## 例：クリックイベントを渡す

子：

    function MyComponent(props) {
      return <button onClick={props.onClick}>Click me</button>
    }

親：

    function App() {
      const handleClick = () => {
        alert("Button was clicked!")
      }

      return <MyComponent onClick={handleClick} />
    }

---

# 関数コンポーネントの使い方

関数コンポーネントはタグとして呼び出します。

例：

    function ComponentName() {
      return (
        <div>
          <h1>Hello, World!</h1>
        </div>
      )
    }

使用：

    function App() {
      return (
        <div>
          <ComponentName />
        </div>
      )
    }