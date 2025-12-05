---
layout: post
title: "React component"
date: 2025-12-05
categories: [JavaScript]  
permalink: /React-compo/
---

## props（プロップス）    
親コンポーネントから子コンポーネントに渡すデータ  
子は props.名前 で受け取って使う 


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
```
## props.children  


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
