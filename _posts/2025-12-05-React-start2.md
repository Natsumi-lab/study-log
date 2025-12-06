---
layout: post
title: "React入門編"
date: 2025-12-04
categories: [React]  
permalink: /react-start2/
---
## コンポーネント  
React では UI を「パーツ（部品）」として作ります  
例えば：  
Header（ヘッダー）  
Footer（フッター）  
Button（ボタン）  
UserCard（ユーザー情報カード）  
これらの UI 部品 = コンポーネント です  

```
コンポーネントごとにファイル分けすると…
Button.jsx
export default function Button() {
  return <button>クリック</button>;
}

Header.jsx
export default function Header() {
  return <header>ヘッダー</header>;
}

React でコンポーネントを別ファイルに分ける理由は：
✔ 1つのファイルが長くならない
✔ どこに何があるかわかりやすい（可読性アップ）
✔ 部品として使い回せる（再利用しやすい）
✔ メンテナンスしやすい
---------------------------
functionは関数を定義するためのキーワード
ComponentNameはコンポーネントの名前です。関数コンポーネントの名前は大文字で始めるのが一般的。
利用したい箇所で関数コンポーネント<ComponentName />と記述することで関数コンポーネントを利用することができます。

function ComponentName() {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
}

function App() {
  return (
    <div>
      <ComponentName />
    </div>
  );
}

---------------------------
関数コンポーネントはアロー関数を使って以下のように書くこともできます
const MyComponent = () => {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
}

function App() {
  return (
    <div>
      <MyComponent />
    </div>
  );
}

```

## createRoot/root.render  
createRootは画面の特定の部分を管理するための準備をする  
root.renderはその部分に実際に表示したい内容を描き出します  
```
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
↓
例:ウェブページの初期表示
ウェブページを初めて表示するときに、どの部分に何を表示するかを決めるために使います。createRootで表示する場所を決め、root.renderでその場所に内容を表示します。
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello, World!</h1>);

ReactDOM.createRootは、表示したい場所を指定して準備を整えるためのキーワードです。
document.getElementById('root')は、HTMLの中で表示したい場所を特定するための方法です。
root.renderは、準備が整った場所に実際に表示する内容を描き出すためのキーワードです。
<App />は表示したい内容を指しています。
```

## JSX   
```
HTMLのように見た目を記述しつつ、JavaScriptの力を使って動きを加えることができます  
const element = <h1>Hello, world!</h1>;  

関数コンポーネントと組み合わせることも可能  
const ComponentName = () => {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
};
```
## {} はJSX の中に JavaScript を書くための「カッコ」  
HTML のように見える JSX の中には、本来 JavaScript はそのまま書けません。  
そこで、**{} に入れた部分だけ JavaScript として扱える**というルールがあります。  
<button onClick={props.onClick}>Click me</button>  
「props.onClick という JavaScript の値（関数）を onClick に渡します」という意味。  

## React.Fragment  
**<></> は <React.Fragment></React.Fragment> の短縮形**  

**<tag />　半角スペースに/**  
中身がないとき <img /> など、自己閉じタグが一般的  

```
①　
<Text>
  ここに何か入れる
</Text>

②
<Text />　
半角スペースに/

①と②は全く同じ意味。
タグの中に何も入れるものがなければ<Test />と書くのが普通。
--------------------------------
余計な div を入れた場合
jsx
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);

Fragment を使った場合～一番よく使われるショート記法 <>
jsx
return (
  <>
    <h1>Hello</h1>
    <p>World</p>
  </>
);
----------------------
ショート記法でない場合
return (
  <React.Fragment>
    <h1>Hello</h1>
    <p>World</p>
  </React.Fragment>
);
```

##  フック関数  
関数コンポーネントの中で “状態を持ったり、特別な動きを追加するため” の仕組み  
例えば：　　
カウンターの数字を覚えておく、入力フォームの文字を覚えておく、ボタンを押した回数を記録する  
**その代表が useState**    
フックはすべて use から始まる名前 をしています（例：useState, useEffect） 
コンポーネントのトップレベル（＝ return の前の、if やループの外）に書く  
```
const [state, setState] = useState(initialState);

state     → 今の値（状態）
setState  → 値を更新するための関数
initialState → 初期値（最初の状態）
----------------
例：ボタンをクリックするたびにsetCountが呼ばれ、countが1ずつ増加

import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>現在のカウント: {count}</p>
      <button onClick={() => setCount(count + 1)}>カウントアップ</button>
    </div>
  );
}
``` 
## useEffect  
ウェブページが表示されたときや、特定のデータが変わったときに自動的に何かをしたい場合に使うフック関数の一つです。特定のタイミングで指定した動作をするように設定することができます。  
```
useEffect(() => {
  // ここに実行したいコードを書く
}, [依存する値]);
```
- useEffectは、最初の引数に実行したい関数を、第二の引数にその関数を実行するタイミングを決めるための値のリストを渡します。このリストの中の値が変わると、関数が実行されます。この例では、最初の引数をアロー関数で指定しています。    

- useStateにおいて、状態を更新する関数を実行した後、更新された状態を直ちに参照することはできませんが、useEffectを使うことで更新された状態を使って処理を行うことができます。   
``` 
 import { useEffect, useState } from 'react';

switchState（スイッチの状態） という変数を使えるようにしています
初期値 → 'off'
function App() {
  const [switchState, setSwitchState] = useState('off') // 初期値 off

prevState は React が持っている直前の最新の状態
もし前の状態が off なら on にする  
もし前の状態が on なら off にする
  const toggleSwitch = () => {
    setSwitchState((prevState) => (prevState === 'off' ? 'on' : 'off'));
  }

switchState が変わるたび、つまり
off → on に変わったとき
on → off に変わったとき
console に現在の状態が表示される。
  useEffect(() => {
    console.log(switchState);
  }, [switchState]);

  return (
    <>
      <p>現在の状態：{switchState}</p>
      <button onClick={toggleSwitch}>
        スイッチ
      </button>
    </>
  )
}
```
## カスタムフック  
- use○○ という名前で自分で作る関数のこと  例：useUser, useFetch, useToggle  
- useState や useEffect などのフックを組み合わせる  
- よく使う処理をまとめたい、複数のコンポーネントで同じロジックを使い回したいときに使う  
- フックを通常の関数と同じ場所ではなくコンポーネントの外で呼び出す
（React のルールで、フックはコンポーネント or カスタムフック内でしか使えません。）
```
function useCustomHook() {
  const [state, setState] = useState(initialValue);

  useEffect(() => {
    // 何らかの副作用処理
    return () => {
      // クリーンアップ処理
    };
  }, [dependencies]);

  return [state, setState];
}
-----------------------

カスタムフックとして切り出す
// useMousePosition.js
import { useState, useEffect } from "react";

export const useMousePosition = () => {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  useEffect(() => {
    const update = e => setPosition({ x: e.clientX, y: e.clientY })
    window.addEventListener('mousemove', update)
    return () => window.removeEventListener('mousemove', update)
  }, [])

  return position
}
---------------
コンポーネント側はとてもスッキリ！
ロジックが完全に分離され、読みやすくなりました。
const App = () => {
  const position = useMousePosition()

  return (
    <div>
      マウス位置: {position.x}, {position.y}
    </div>
  )
}
```
## 分割代入  
カスタムフックでよく使われる書き方  
```
handleIncrement とは状態（count）を 1 増やすための関数

const { count, handleIncrement } = useCounter()

上記は次と同じ意味です：
const result = useCounter()
const count = result.count
const handleIncrement = result.handleIncrement
```

## 条件付きレンダー/三項演算子  
{ condition ? <Component1 /> :<Component2 /> }  
```
例：商品の在庫がある場合は購入ボタンを表示し、在庫がない場合は「売り切れ」のメッセージを表示
 const isInStock = false;

function ProductButton() {
  return (
    <>
      { isInStock ? <button>購入する</button> : <span>売り切れ</span> }
    </>
  );
}
```

## prevState  
React の useState において、状態の更新時に「直前」の状態を参照するための値  
```
setState((prevState) => {
  // 新しい状態を計算する
  return newState;
});

---------------
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  const incrementCount = () => {
    setCount((prevState) => prevState + 1)
    setCount((prevState) => prevState + 1)
  }

  return (
    <>
      <p>現在のカウント: {count}</p>
      <button onClick={incrementCount}>カウントを増やす</button>
    </>
  );
}
```
---  
## setState の 2つの書き方の違い  

### ① setCount(count + 1) ← 古い値を使って計算する書き方 
``` 
const incrementCount = () => {
  setCount(count + 1)
  setCount(count + 1)
}

仮に 今の count が 10 だとします。
setCount(10 + 1)
setCount(10 + 1)

つまりどちらも
setCount(10 + 1)
setCount(10 + 1)

実行結果は +2 ではなく +1 です。
count = 10

setCount(10 + 1) → 11 になる予定
setCount(10 + 1) → 11 になる予定

最終結果 → 11
```
state（count）は 同じ関数内では更新されない  
（更新は “あとでまとめて行われる”）  
なので、関数の途中で setCount を何回呼んでもその時点では count はまだ 10 のまま。

------------------------------------

### ② setCount(prevCount => prevCount + 1) ← 常に最新の値を使う書き方  
```
const incrementCount = () => {
  setCount(prevCount => prevCount + 1)
  setCount(prevCount => prevCount + 1)
}

同じく count = 10 のときどうなるか？
1回目：
prevCount = 10
10 + 1 = 11

2回目：
prevCount = 11（1回目で更新された値）
11 + 1 = 12

だから最終結果は 12。
```
つまりこの2つの違い  
| 書き方                                    | 使われる値                | 結果                 |
| -------------------------------------- | -------------------- | ------------------ |
| `setCount(count + 1)`                  | 「関数が開始した時点の count」だけ | +1 にしかならない         |
| `setCount(prevCount => prevCount + 1)` | 毎回「最新の count」        | 正しく +2, +3… と加算される |


## stateの更新+スプレッド構文  
```
例1：配列に要素を追加
/** 配列の場合 */
const [sweetsList, setSweetsList] = useState(["cake", "caramel"])
console.log(sweetsList) //["cake", "caramel"]

// 要素を追加する
setSweetsList([...sweetsList, "cookie"])
console.log(sweetsList) //["cake", "caramel", "cookie"]


例2：オブジェクトのプロパティ更新と追加
/** オブジェクトの場合 */
const [user, setUser] = useState({
  name: "Takeshi",
  age: 25
})
console.log(user) //{ name: "Takeshi", age: 25 }

setUser({
  ...user, 
  age: 30
})
console.log(user) //{ name: "Takeshi", age: 30 }

setUser({
  ...user, 
  gender: "male" 
})
console.log(user) //{ name: "Takeshi", age: 25, gender: "male" }
```

## イベントハンドラ  
ユーザーがボタンをクリックしたり、テキストを入力したりするような状況で、これらの操作に応じて処理を実行させる仕組みです。操作に対応させる処理の関数を定義し、要素にその関数を関連付けて利用します。  

<button onClick={handleClick}>Click me</button>  

- onClickはイベントハンドラのクリックに対応する処理を指定するキーワードです。ボタンがクリックされたときに、handleClickという関数を実行するように設定しています。  
- handleClickの処理はここでは記述していませんが、例えば銀行の振込ボタンであれば振込実行の処理を関連付けたり、ゲームの攻撃ボタンであれば攻撃する処理を紐づけたりします。  
```
例1: ボタンのクリックイベント（onClick）
function handleClick() {
  alert('ボタンがクリックされました！');
}

<button onClick={handleClick}>Click me</button>

----------------

例2: テキスト入力の変更イベント（onChange）
入力されたテキストを画面に表示することができます。
handleChangeという関数が定義されており、テキストボックスの内容が変更されるたびに、その内容がコンソールに表示されます。event.target.valueは入力されたテキストの現在の値を取得します。

function handleChange(event) {
  console.log(event.target.value);
}

<input type="text" onChange={handleChange} />
```
### React の主なイベントハンドラ一覧  
先頭が小文字のキャメルケース  
| **イベントハンドラ**    | **実行タイミング**            |
| --------------- | ---------------------- |
| `onChange`      | 対象の入力欄の値が変更された時        |
| `onClick`       | 対象の要素をクリックした時          |
| `onDoubleClick` | 対象の要素をダブルクリックした時       |
| `onMouseMove`   | マウスのカーソルが移動した時         |
| `onMouseEnter`  | マウスのカーソルが対象の要素の中に移動した時 |
| `onMouseLeave`  | マウスのカーソルが対象の要素の外に移動した時 |
| `onMouseDown`   | 対象の要素の中でマウスのボタンが押された時  |
| `onMouseUp`     | 対象の要素の中でマウスのボタンが離された時  |
| `onKeyDown`     | キーが押された時               |
| `onKeyUp`       | キーが離された時               |
| `onKeyPress`    | （非推奨）文字列を生成するキーが押された時  |
| `onFocus`       | 対象の要素がフォーカスされた時        |
| `onBlur`        | 対象の要素からフォーカスが外れた時      |
| `onSubmit`      | フォームの送信操作が実行される時       |
| `onScroll`      | スクロールした時               |
| `onLoad`        | 必要なリソースが全て読み込まれた時      |
| `onUnload`      | リソースがアンロードされる時         |

## インラインスタイル  
ウェブページの見た目を変えたいときに、特定の要素に直接スタイルを適用する方法  
```
const style = {
  color: 'blue',
  fontSize: '20px'
};

function MyComponent() {
  return <div style={style}>Hello, world!</div>;
}
```







