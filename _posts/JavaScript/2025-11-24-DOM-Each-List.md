---
layout: post
title: "DOM～配列処理・NodeList・getElementとaddEventの整理"
date: 2025-11-23
categories: [JavaScript / DOM]  
permalink: /DOM-Each-List/
---
【内容】

- forEach の使い方
- NodeList と配列の違い
- getElementById と addEventListener の使い分け  

------------------------------------------------------------
■ 配列に対する forEach（基本）  
------------------------------------------------------------

【どんなときに使う？】
・配列の全ての要素に同じ処理をしたいとき

【書き方】配列.forEach(関数)  
```
const names = ["Alice", "Bob", "Charlie"];

names.forEach(function(name) {
  console.log(name);
});
```

【特徴】
・1つずつ順番に値が取り出される
・配列の定番ループ方法


------------------------------------------------------------
■ NodeList でも forEach が使える
------------------------------------------------------------

【ポイント】
document.querySelectorAll() が返すのは「NodeList」
NodeList は「配列ではない」が forEach が使える。

【どんなときに使う？】
・複数の DOM 要素に同じ操作をしたいとき

【書き方】
```
// .item クラスの要素を全て取得
const items = document.querySelectorAll(".item");

// NodeList だが、forEach が使える
items.forEach(function(el) {
  el.style.color = "red";
});
```

【特徴】
・配列とほぼ同じ感覚でループできる
・HTMLCollection（getElementsBy〜 の戻り値）では直接 forEach は使えない


------------------------------------------------------------
■ NodeList と 配列 の違い（最低限の理解）
------------------------------------------------------------

【NodeList】
・querySelectorAll() で返ってくる
・forEach が使える
・基本 static（DOM が変化しても中身は変わらない）

【配列】
・すべての配列メソッドが使える（map, filter など）

【HTMLCollection（参考）】
・getElementsByClassName / TagName で返る
・forEach はそのまま使えない
・自動更新される（ライブコレクション）


------------------------------------------------------------
■ getElementById と addEventListener の使い分け
------------------------------------------------------------
【1】getElementById は「要素を取得する」ためのもの
------------------------------------------------------------

【役割】
・HTML の中から「特定の要素」を取り出すだけ

【例】
```
const btn = document.getElementById("myButton");
```

【できること】
・その要素の textContent や value を読む
・CSS を変える
・イベントを登録する（addEventListener と組み合わせる）


------------------------------------------------------------
【2】addEventListener は「反応させる」ためのもの
------------------------------------------------------------

【役割】
・クリックされたとき
・入力が変わったとき
・ページの読み込みが終わったとき
など、「イベントが起きたときの処理」を登録する

【例】
```
btn.addEventListener("click", function() {
  console.log("クリックされました");
});
```


------------------------------------------------------------
【3】結論：両方セットで使うのが普通
------------------------------------------------------------

getElementById → 対象を取得
addEventListener → その要素に反応させる処理を登録

【セットの例】
```
const button = document.getElementById("myButton");

button.addEventListener("click", function() {
  console.log("クリックされた！");
});
```


------------------------------------------------------------
■ よくある間違い例
------------------------------------------------------------

【間違い1】addEventListener だけで要素を扱おうとする  
→ addEventListener は「要素が必要」  
　先に getElementById / querySelector で取得しておく必要がある

【間違い2】getElementById を毎回呼ぶ  
→ DOM を何度も検索するので効率が悪い  
　変数に入れて使い回すほうがよい


------------------------------------------------------------
■ ここまでのまとめ
------------------------------------------------------------

【forEach】
・配列に使う基本のループ
・NodeList にも使える
・HTMLCollection には使えない

【NodeList】
・querySelectorAll の結果
・配列っぽいが完全な配列ではない

【getElementById と addEventListener】
・前者は「要素を見つける」
・後者は「反応をつける」
・基本はセットで使用する
