---
layout: post
title: "DOMの基本操作～スタイル・属性・要素の追加/削除"
date: 2025-11-23
categories: [JavaScript / DOM]  
permalink: /DOM-basics/
---

このページでは、取得した DOM 要素に対して
「どんな操作ができるのか」をまとめています。

CSS の変更、クラス付け、属性操作、要素の生成・削除など、  
Web ページを動的に変化させる基本はここに集まっています。 
 
---
【内容】 DOMに対する基本的な操作  
- element.style
- className / classList
- getAttribute / setAttribute / removeAttribute
- readonly / disabled の扱い
- createElement
- appendChild
- remove
- removeChild
---

■ element.style （直接スタイルを変更する）

【どんなときに使う？】
・特定の要素の見た目を直接変えたいとき
・クラス操作ではなく、値をその場で変更したいとき

【何ができる？】
・文字色、背景色、幅、高さ、位置など CSS と同じものを変更できる

【書き方】
```
element.style.color = "red";
element.style.fontSize = "20px";
element.style.backgroundColor = "yellow";
```

【特徴】
・CSS を直接書き換える
・複雑な見た目の変更はクラス操作のほうが管理しやすい

------------------------------------------------------------

■ className （クラスをまるごと上書き）

【どんなときに使う？】
・今のクラスをすべて別のクラスに置き換えたいとき

【何ができる？】
・ class="box active" → class="box hidden" のように、完全に置き換え

【書き方】
```
element.className = "box hidden";
```

【特徴】
・既存のクラスが全部消えるため注意
・まるごと変更したいときにのみ使う

------------------------------------------------------------

■ classList （推奨：クラスを1つずつ追加・削除）

【どんなときに使う？】
・クラスを部分的に追加・削除・切り替えたいとき
・className のように全て上書きしたくないとき

【何ができる？】
- add()：クラスの追加
- remove()：クラスの削除
- toggle()：付いていれば削除、無ければ追加、メニューの開閉でとてもよく使う
- contains()：持っているか確認
- replace()：クラス名の置換

【書き方】
```
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("open");
element.classList.contains("active");
element.classList.replace("small", "large");
```

【特徴】
・もっとも安全で使いやすいクラス操作方法
・現代の開発では classList が基本

------------------------------------------------------------

■ getAttribute / setAttribute / removeAttribute （属性操作）

【どんなときに使う？】
・HTML の属性を変更したいとき
例：href、id、src、title、data-* 属性など

【何ができる？】
・属性の追加、変更、削除

【書き方】
```
element.getAttribute("id");
element.setAttribute("data-info", "test");
element.removeAttribute("readonly");
```

【特徴】
・HTML の属性を自由に変更できる
・classList や style のような専用APIが無い属性に使う

------------------------------------------------------------

■ readonly（入力の変更を禁止する）

【どんなときに使う？】
・ユーザーに入力させたくないが、見た目はそのままにしたい

【書き方】
```
const input = document.getElementById("input");
input.setAttribute("readonly", true);
```

【特徴】
・値の表示はできるがユーザーは書き換えできない

------------------------------------------------------------

■ disabled（無効化する）

【どんなときに使う？】
・ボタンを押せなくしたい
・入力欄を操作不可にしたい

【書き方】
```
const btn = document.getElementById("button");
btn.setAttribute("disabled", true);
```

【特徴】
・ボタンは押せなくなり、入力欄は触れなくなる

------------------------------------------------------------

■ createElement（新しい要素を作る）

【どんなときに使う？】
・新しい HTML の要素を JavaScript で作りたいとき

【何ができる？】
・div や p など好きなタグを生成できる

【書き方】
```
const newDiv = document.createElement("div");
```

【特徴】
・作っただけでは表示されないため appendChild とセットで使う

------------------------------------------------------------

■ appendChild（要素を追加する）

【どんなときに使う？】
・作った要素や既存の要素を他の要素の中に追加したいとき

【書き方】
```
parent.appendChild(newDiv);
```

【特徴】
・親要素の一番最後に追加される

------------------------------------------------------------

■ remove（要素を削除）

【どんなときに使う？】
・対象の要素を完全に削除したいとき

【書き方】
```
element.remove();
```

【特徴】
・その要素自身が消える
・もっとも簡単な削除方法

------------------------------------------------------------

■ removeChild（子要素を削除）

【どんなときに使う？】
・親要素から特定の子だけを削除したいとき

【書き方】
```
parent.removeChild(child);
```

【特徴】
・「親要素」が必要
・remove() よりも厳密に要素を削除したいときに使う

------------------------------------------------------------
