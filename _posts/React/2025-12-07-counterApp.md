---
layout: post
title: "React カウンターアプリ"
date: 2025-12-07
categories: [React]  
permalink: /counter-app/
---
Reactで作ったカウンターアプリです。  
各コードがどんな動きをしているかコメントアウトを付けました。  

```
// ReactDOM: Reactコンポーネントをブラウザの画面に描画するためのライブラリ
import ReactDOM from 'react-dom'
// React と useState フックを読み込む
import React, { useState } from 'react'

// App というコンポーネント（部品）を作成
const App = () => {
  // count という状態変数と、それを更新する setCount 関数を作成
  // useState(0) により初期値は 0
  const [count, setCount] = useState(0)

  // プラスボタンが押された時に呼ばれる関数
  const handleIncrease = () => {
    // 現在の count に 1 を足す
    setCount((count) => count + 1)
  }

  // マイナスボタンが押された時に呼ばれる関数
  const handleDecrease = () => {
    // 現在の count から 1 を引く
    setCount((count) => count - 1)
  }

  // 2倍ボタンが押された時に呼ばれる関数
  const handleDouble = () => {
    // 現在の count を2倍にする
    setCount((count) => count * 2)
  }

  // リセットボタンが押された時に呼ばれる関数
  const handleReset = () => {
    // count の値を 0 に戻す
    setCount(0)
  }

  // ここから画面(UI)として描画される内容
  return (
    <div>
      {/* 現在の count の値を画面に表示 */}
      <h1> カウント{count} </h1>

      {/* プラスボタン。クリック時に handleIncrease を実行 */}
      <button style={{ fontSize: '20px' }} onClick={handleIncrease}>
        プラス
      </button>

      {/* マイナスボタン */}
      <button style={{ fontSize: '20px' }} onClick={handleDecrease}>
        マイナス
      </button>

      {/* 2倍ボタン */}
      <button style={{ fontSize: '20px' }} onClick={handleDouble}>
        ２倍
      </button>

      {/* リセットボタン */}
      <button style={{ fontSize: '20px' }} onClick={handleReset}>
        リセット
      </button>

      {/* count が 10 以上のときだけ表示されるメッセージ */}
      {count >= 10 && (
        <div style={{ fontSize: '24px', marginTop: '40px' }}>
          10以上になりました。
        </div>
      )}

      {/* count が 0 以下のときだけ表示されるメッセージ */}
      {count <= 0 && (
        <div style={{ fontSize: '24px', marginTop: '40px' }}>
          0以下になりました。
        </div>
      )}
    </div>
  )
}

// 作成した App コンポーネントを HTML の #root に描画する
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

# ポイント  
## useStateとは？  
React で 「状態を持つ」ための仕組み  
count に値が入り、setCount でその値を更新すると自動で画面が再描画される  

## 条件付きレンダリング {count >= 10 && (...)} の意味  
これは JavaScript の 論理AND（&&） を利用した書き方  
条件 && <表示したい内容>  
条件が true のときだけ 右側の要素が画面に出る  