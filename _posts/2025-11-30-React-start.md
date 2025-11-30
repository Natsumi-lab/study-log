---
layout: post
title: "React入門編"
date: 2025-11-30
categories: [React]  
permalink: /react-start/
---

let  
const  

```
// オブジェクトのプロパティ値を変更・追加する
// オブジェクトの定義
const fruit = {
  name: 'レモン',
  stock: 20
}
console.log(fruit) // {name:'レモン', stock:20}

// プロパティ値を変更
fruit.name = 'グレープフルーツ'
console.log(fruit) // {name:'グレープフルーツ', stock:20}

// プロパティを新たに追加
fruit.color = 'pink'
console.log(fruit) // {name:'グレープフルーツ', stock:20, color:'pink'}

// 配列の定義
const basket = ['いちご', 'さくらんぼ']
console.log(basket) // ['いちご', 'さくらんぼ']

// 1つ目の値を変更
basket[0] = 'マンゴー'
console.log(basket) // ['マンゴー', 'さくらんぼ']

// 配列に値を追加する
basket.push('ライチ')
console.log(basket) // ['マンゴー', 'さくらんぼ', 'ライチ'];
```

関数とは、いくつかの処理をまとめたものです。  
関数宣言の特徴は、コード順番に関係なく実行されます。  
function 関数名 (仮引数1, 仮引数2) {実行処理}  

引数が1つの場合()を省略できる  
constdouble = count => {  return count * 2 };  

処理が1文の場合、returnも省略できる（書かない）  
const double = count => count * 2;  

```
「分割代入」とは、オブジェクトや配列から値を取り出して別個の変数に代入するための方法です。  
// 配列の定義
const myFavoriteAnimal = ['ロッキー', 'dog']

// 配列の分割代入
const [name, type] = myFavoriteAnimal

// 分割代入を使用した結果
console.log(name) // 'ロッキー'
console.log(type) // 'dog'
```