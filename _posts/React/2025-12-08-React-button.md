---
layout: post
title: "Reactでボタンを作る"
date: 2025-12-08
categories: [React]  
permalink: /React-button/
---
Reactでクリックする度に色が変わるボタンをつくります  

```
// 画面へ React コンポーネントを描画するライブラリ
import ReactDOM from 'react-dom'
// React 本体と、状態管理に使う useState を読み込む
import React, { useState } from 'react'

// アプリ全体のコンポーネント
const App = () => {
  // ボタンの状態（true/false）を保持する state
  // true → ON（緑色） / false → OFF（赤色）
  const [isActive, setIsActive] = useState(true)

  // ボタンがクリックされたときに呼ばれる関数
  const handleChangeColor = () => {
    // isActive の値を反転させる（true → false、false → true）
    setIsActive(!isActive)
  }

  return (
    <div
      style={{
        display: 'flex',             // 子要素をflexレイアウトに
        alignItems: 'center',        // 縦方向中央寄せ
        justifyContent: 'center',    // 横方向中央寄せ
        height: '100vh'              // 画面全体の高さ
      }}
    >
      <button
        style={{
          // 文字色：isActive の状態で切り替え
          color: isActive ? '#fff' : '#000',

          // 背景色：isActive が true なら緑、false なら赤
          backgroundColor: isActive ? '#008000' : '#ff0000',

          width: '150px',
          borderRadius: '24px',
          padding: '10px',
          cursor: 'pointer'
        }}

        // クリック時に handleChangeColor が実行される
        onClick={handleChangeColor}
      >
        {/* ボタンの文字も状態に応じて変更 */}
        {isActive ? 'ON' : 'OFF'}
      </button>
    </div>
  )
}

// App コンポーネントを HTML の #root に描画
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

## ポイント解説  
■ 状態が変わると UI が自動で再描画される  

setIsActive(!isActive) で状態を更新すると、  
React が自動でボタンの色・文字を更新してくれる。  

その理由は、style の中や JSX の中で isActive を使っているからです。  

■ 三項演算子の使い方  
isActive ? '#008000' : '#ff0000'  


意味は：  
isActive が true → 左側の値を使う  
isActive が false → 右側を使う  
React の UI 切り替えでよく使われる書き方です。  