---
layout: post
title: "React component"
date: 2025-12-05
categories: [React]  
permalink: /React-compo/
---

## props（プロップス）    
親コンポーネントから子コンポーネントに渡すデータ  
子は props.名前 で受け取って使う 

普通の props（数値・文字列）は 親 → 子の一方向のみ。
しかし関数を渡すと子がその関数を呼ぶことで、親にイベントを伝えられる(子→親のやり取りができる唯一の方法)  
```
子から親へ関数を渡す例：

③ 子コンポーネントは props として受け取る
function MyComponent(props) {
  return <button onClick={props.onClick}>Click me</button>;
}

① 親コンポーネントが関数を持っている
function App() {
  const handleClick = () => {
    alert("Button was clicked!");
  };

② 親が子に“関数そのもの”を渡す
  return <MyComponent onClick={handleClick} />;
}

つまり、-------  

App（親）
 │
 │  onClick={handleClick}  ← 関数を渡す
 ▼
MyComponent（子）

MyComponent がボタンをクリックしたら
props.onClick() を実行

→ 親の handleClick() が呼ばれる！
```
## props の分割代入  
props の中から必要なデータだけを取り出して、変数として扱いやすくする書き方  
```
props の通常の書き方
function MyComponent(props) {
  return <p>{props.message}</p>;
}
この書き方だと、props.message  props.onClick  props.count  のように、毎回 props. と書く必要があります。  
--------------------------------------
props の分割代入
function MyComponent({ message }) {
  return <p>{message}</p>;
}

function MyComponent({ message })
と、下記は同じ意味
const message = props.message;
```
| 方法                     | 説明                         |
| ---------------------- | -------------------------- |
| `props.message`        | そのまま使う従来の書き方               |
| `{ message }`          | props から message を取り出す分割代入 |
| `{ message, onClick }` | 複数の props を取り出せる           |

```
const App = () => {
  return (
    <>
      <h1>ボタン</h1>

      <Button
        text={'Hello World'}
        showLog={() => console.log('クリックしました')}
      />
    </>
  )
}
-----------------------
children という名前は props.children の略
props の分割代入と、props.childrenを合わせた例：

function ChildComponent({ children }) {
  return <div>{children}</div>;
}
```
## props.children  
子コンポーネントを「枠（レイアウト）」として使える  
再利用できる「枠」コンポーネントが作れる  
```
function ParentComponent() {
  return (
    <ChildComponent>
      <p>この部分がprops.childrenとして渡されます。</p>
    </ChildComponent>
  );
}

function ChildComponent(props) {
  return (
    <>
      {props.children}
    </>
  );
}

つまり、-------  

ParentComponent
 ↓ 中に書いた内容
<ChildComponent>
    ★ この部分が props.children ★
</ChildComponent>

ChildComponent（子）
 ↓ props.children を使う
{props.children} をここに表示
----------------  

例: デフォルトコンテンツの設定
タイトルが無ければ「タイトルなし」
function Title({ text }) {
  return <h1>{text || "タイトルなし"}</h1>;
}

<Title text="こんにちは" />   // → こんにちは
<Title />                   // → タイトルなし
```
## A || B   JavaScriptの推論  
A が 空 / undefined / null / false → B を採用
A に 値がある → A を採用
つまり左がなかったら右を使う   

------------------------------  

## 「親コンポーネント」「子コンポーネント」とは？  
✔ 親コンポーネント  
→ 中に他のコンポーネントを含んでいるコンポーネント  

✔ 子コンポーネント  
→ 親の中に書かれているコンポーネント  
```
function ParentComponent() {
  return <ChildComponent />;
}

例1: データの表示
const ParentComponent = () => {
  const user = {
    name: "太郎",
    age: 25
  };
  return <ChildComponent name={user.name} age={user.age} />;
};

const ChildComponent = (props) => {
  return (
    <div>
      <p>名前: {props.name}</p>
      <p>年齢: {props.age}</p>
    </div>
  );
};

-----------------
例2: ボタンのクリックイベント
function MyComponent(props) {
  return <button onClick={props.onClick}>Click me</button>;
}

function App() {
  const handleClick = () => {
    alert("Button was clicked!");
  };

  return <MyComponent onClick={handleClick} />;
}

```
