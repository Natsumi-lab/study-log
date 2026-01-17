---
layout: post
title: "非同期処理"
date: 2025-11-22
categories: [JavaScript]  
permalink: /JavaScript/  
---
まず、プログラムはコードの上から順に実行していくものであるから、  
上の処理が終わらないと次にいけないという前提がある   

# 非同期処理とは  
- 実行完了を待たずに、次の処理へ進むこと  
- 並行して次の処理を実行する   
　 例：画像が表示されるまで時間がかかるので、テキストだけ先に表示しておく   
- ユーザーを待たせないことがメリット  
<br>


## 非同期になるときとは
通信が発生する処理で起こる  
　例：  
- web APIを叩くときや、データベースへクエリを投げるとき  
- fetch 通信は時間がかかるため  
- setTimeout（コールバック関数）指定時間まで待つあいだ停止しない。  
  コールバック関数は、革命的な非同期処理だったがネストが多く読みにくくなる。コールバック地獄となる。
<br>

## 同期処理と非同期処理の比較  
A→B
A←B

同期処理は:  
AはBに処理を渡してBからレスポンスが返ってくるまで、Aは待つ（処理に時間がかかる）  

非同期は:  
AはBに処理を渡している間にも他の仕事を継続してやる。  
Bからレスポンスが返ってくれは当然それもやる。（処理が速い）  
<br>

## 完了を待つ方法は２つある  
これらは、コールバック関数の後に誕生した非同期処理  
方法１） **Promise** で待つ  
    【Promiseの状態】  
    - pending:初期状態  
    - fulfilled：処理が完了して完了した状態  
    - rejected:処理が失敗して完了した状態  
  
```
// ↓resolve又はrejectが返ってくるまで処理を待ちます
const promiseFunc= () => {
    return new Promise((resolve, reject) => {
            // 何らかの非同期処理
        }).then(() => }
            // 非同期処理が成功した場合
            return resolve('成功')
        }).catch(() => {
            // 非同期処理が失敗した場合
            return reject('失敗')
        })
    })
}
```

Promiseの誕生によりネストは解消されたが、thenが多くてコードが読みにくくなる  
→　それを解消するのがasync/await✨    

方法２） **async/await** で待つ  
Promiseより記述がシンプル    
関数を定義するときにasyncをつける   
関数を実行するときにawaitを付ける    

※awaitはasync付き関数内でしか使えない
```
const asyncAwaitFunc = async () => {
    const demo = await someAsynchoronousFunc(() => {
            // 何らかの非同期処理
        }).then(() => {
            // 非同期処理が成功した場合
            return '成功'
        }).catch(() => {}
            // 非同期処理が失敗した場合
            return '失敗'
        })
}   
```
async/awaitは、thenを使わなくていいので、コードが普通に書ける✨  
**という訳で非同期処理はasync/awaitがオススメ**