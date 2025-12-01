---
layout: post
title: "React入門編"
date: 2025-11-30
categories: [React]  
permalink: /react-start/
---

# ここではReactに良く出るJavaScriptのおさらいをします  

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

## スプレッド構文   ...（ドッド3つ） をつなげて書く  
スプレッド構文を使うことで、配列やオブジェクトの要素を一つずつ取り出して、1つ1つ別々のものとして扱うことができます  
これは、配列やオブジェクトを簡単にコピーしたり、要素を追加したりする場合に非常に便利  
```
配列のコピーを作成
const numbers = [10, 20]
const copyNumbers = [...numbers]

console.log(copyNumbers) // [10, 20]


配列に要素を追加して新しい配列を作成
const numbers = [10, 20]
const updatedNumbers = [...numbers, 30, 40]

console.log(updatedNumbers) // [10, 20, 30, 40]


オブジェクトのコピーを作成
const user = { id: 1, age: 20 }
const copyUser = {...user}

console.log(copyUser) // { id: 1, age: 20 }


オブジェクトに要素を追加して新しいオブジェクトを作成
const user = { id: 1, age: 20 }
const updatedUser = {
  ...user,
  job: 'teacher'
}

console.log(updatedUser) // { id: 1, age: 20, job: 'teacher' }


// 関数での使用例
const array = [2, 5]
const addFunction = (a, b) => {
   return console.log(a + b)
}

addFunction(...array) // 出力結果が、7となる
```

## 配列操作/map  
mapメソッドは、呼び出し元の配列の各要素に対して処理を行い、元の配列を変更せずに新しい配列を作ります 

```
let 新しい配列 = array.map((element, index, array) => {
  // 処理
  return element;
});
element: 現在処理中の要素
index: 現在処理中の要素のインデックス（省略可能）
array: 元の配列（省略可能）


配列usersの各要素のnameプロパティの値の先頭にMr.を追記する
const users = [
  { name: 'Williams' },
  { name: 'Brown' }
];

console.log(users.map((user) => 'Mr. ' 
```

## 配列操作/filter(プリミティブ)   
 
