---
layout: post
title: "DOM～よく使うイベントとフォーム操作"
date: 2025-11-23
categories: [JavaScript / DOM]  
permalink: /DOM-event-and-form/
---
このページでは、実際によく使うイベントについてまとめます。

- クリックされたとき（click）  
- ページの読み込みが終わったとき（DOMContentLoaded）  
- 入力内容が変わったとき（change）
- フォームを送信しようとしたとき（submit）
- スクロールしたとき（scroll）

また、フォームの値を読む・書く、送信を止める、ボタンを無効化する、
といった操作もあわせて整理します。  

【内容】
- DOMContentLoaded と load の違い
- click / mouseover / mouseout / scroll
- change / submit
- preventDefault
- value操作
- disabled
- reset
- removeEventListener

------------------------------------------------------------
■ DOMContentLoaded（ページの読み込みが終わったとき）

【どんなときに使う？】
・HTML が読み込まれて、要素が使える状態になってから処理を実行したいとき。

【ポイント】
・画像などすべての読み込みが終わるのを待つ「load」とは別。
・「DOM（要素）」だけ使えればよい場合は、DOMContentLoaded で十分。

【書き方】
```
document.addEventListener("DOMContentLoaded", function() {
  console.log("HTML の読み込みが完了しました");
  const title = document.getElementById("title");
  // ここで DOM を安全に操作できる
});
```

------------------------------------------------------------

■ click イベント（クリックされたとき）

【どんなときに使う？】
・ボタンを押したときに何か処理をしたいとき。

【何ができる？】
・ボタンを押したらメッセージを出す
・メニューを開く
・フォームを送信する など

【書き方】
```
const button = document.getElementById("myButton");

button.addEventListener("click", function() {
  console.log("ボタンがクリックされました");
});
```

------------------------------------------------------------

■ mouseover / mouseout（マウスが乗った・離れた）

【どんなときに使う？】
・マウスを重ねたときに色を変えたい
・ヒント（ツールチップ）を表示したい

【書き方：mouseover】
```
const box = document.getElementById("box");

box.addEventListener("mouseover", function() {
  console.log("マウスが乗りました");
});
```

【書き方：mouseout】
```
box.addEventListener("mouseout", function() {
  console.log("マウスが離れました");
});
```

------------------------------------------------------------

■ scroll イベント（スクロールしたとき）

【どんなときに使う？】
・ページのスクロール量に応じてヘッダーを小さくしたい
・「ページ上部へ戻る」ボタンを表示したい

【書き方】
```
document.addEventListener("scroll", function() {
  console.log("スクロール中です");
  console.log("縦スクロール量:", window.scrollY);
});
```

【スクロール位置を取得・移動する】
```
console.log(window.scrollX); // 横方向のスクロール量
console.log(window.scrollY); // 縦方向のスクロール量

// ページの一番上へ移動
window.scrollTo(0, 0);

// 縦方向 500px の位置まで移動
window.scrollTo(0, 500);
```

------------------------------------------------------------

■ change イベント（入力内容が変わったとき）

【どんなときに使う？】
・入力された値が変わったタイミングで処理したいとき
・セレクトボックスの選択が変わったときに何かしたいとき

【書き方】
```
const textbox = document.getElementById("myTextbox");

textbox.addEventListener("change", function() {
  console.log("値が変わりました:", this.value);
});
```

------------------------------------------------------------

■ submit イベント（フォームが送信されるとき）

【どんなときに使う？】
・フォーム送信前に入力チェックをしたいとき
・JavaScript で送信処理を行いたいとき

【書き方】
```
const form = document.getElementById("myForm");

form.addEventListener("submit", function(event) {
  event.preventDefault(); // ブラウザの標準の送信を止める
  console.log("送信を止めて、自分で処理します");
});
```

------------------------------------------------------------

■ e.preventDefault()（標準の動作を止める）

【どんなときに使う？】
・リンクをクリックしてもページ移動したくないとき
・フォームを送信せずに、自分で処理したいとき

【書き方】
```
// リンクのクリックでページ遷移しないようにする例
const link = document.querySelector("a");

link.addEventListener("click", function(e) {
  e.preventDefault();
  console.log("リンク先には移動しません");
});
```

------------------------------------------------------------

■ value プロパティ（入力値の取得と設定）

【どんなときに使う？】
・テキストボックスの中身を読みたい
・初期値を入れたい

【書き方：値を取得】
```
const input = document.getElementById("input");
const text = input.value;
console.log(text);
```

【書き方：値をセット】
```
input.value = "初期値です";
```

------------------------------------------------------------

■ disabled プロパティ（無効化する）

【どんなときに使う？】
・ボタンを一時的に押せなくしたいとき
・条件を満たすまで入力させたくないとき

【書き方】
```
const button = document.getElementById("submitButton");
button.disabled = true;  // 無効化
// button.disabled = false; // 有効に戻す
```

------------------------------------------------------------

■ フォームの reset（入力を初期状態に戻す）

【どんなときに使う？】
・「リセット」ボタンをクリックしたときにフォームを元に戻したいとき

【書き方】
```
const form = document.getElementById("myForm");

form.reset();
```

------------------------------------------------------------

■ removeEventListener（イベントを解除する）

【どんなときに使う？】
・一度登録したイベントをあとから無効にしたいとき

【ポイント】
・addEventListener で登録したときと「同じ関数」を渡す必要がある。

【書き方】
```
// まずハンドラを用意
function handleClick() {
  console.log("クリックされました");
}

const btn = document.getElementById("myButton");

// イベント登録
btn.addEventListener("click", handleClick);

// あとでイベント解除
btn.removeEventListener("click", handleClick);
```
