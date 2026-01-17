---
layout: post
title: "Reactでパスワードを生成する"
date: 2025-12-09
categories: [React]  
permalink: /React-passGenerator/
---
Reactでパスワードを生成します  
各コードがどの様な動きをしているかコメントアウト付きです  

```
// React コンポーネントを画面に描画するためのライブラリ
import ReactDOM from 'react-dom'
// React 本体と useState フックを読み込む
import React, { useState } from 'react'

function App() {
  // 生成されたパスワードを保持する state
  const [password, setPassword] = useState('')

  // パスワード生成関数
  // length・hasNumbers・hasSymbols をオブジェクトで受け取る
  const generatePassword = ({
    length = 12,         // パスワードの長さ（デフォルト12）
    hasNumbers = true,   // 数字を含めるか
    hasSymbols = true    // 記号を含めるか
  }) => {
    // 各文字セットを定義
    const alpha = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
    const numbers = '0123456789'
    const symbols = '!@#$%^&*_-+='

    // まずは英字をベースにする
    let chars = alpha

    // ユーザーが選択していれば数字を追加
    if (hasNumbers) {
      chars += numbers
    }

    // 記号を含める場合は追加
    if (hasSymbols) {
      chars += symbols
    }

    // ランダムに選んでパスワードを作る
    let newPassword = ''
    for (let i = 0; i < length; i++) {
      // chars の中からランダムに1文字取り出す
      newPassword += chars[Math.floor(Math.random() * chars.length)]
    }

    // state に新しいパスワードを保存 → 画面が更新される
    setPassword(newPassword)
  }

  return (
    <div>
      <h1>パスワードジェネレーター</h1>

      {/* 現在のパスワードを表示 */}
      <p>{password}</p>

      {/* 子コンポーネントに関数を渡す（props） */}
      <PasswordForm generatePassword={generatePassword} />
    </div>
  )
}

function PasswordForm({ generatePassword }) {
  // フォームの入力状態をまとめて管理する state
  const [passwordData, setPasswordData] = useState({
    length: 12,       // パスワードの長さ
    hasNumbers: true, // 数字を含める
    hasSymbols: true  // 記号を含める
  })

  // フォーム送信時の処理
  const handleSubmit = (event) => {
    event.preventDefault() // ページリロードを防ぐ
    generatePassword({ ...passwordData }) // 親の生成関数へデータを渡す
  }

  return (
    <form onSubmit={handleSubmit}>
      {/* パスワードの長さ選択（ラジオボタン） */}
      <label>
        パスワードの長さ：
        <label>
          <input
            type="radio"
            value="12"
            name="length"
            // 選択状態のチェック
            checked={passwordData.length === 12}
            // 入力変化時に state を更新
            onChange={(event) =>
              setPasswordData({
                ...passwordData,                // 既存の値を保持
                length: parseInt(event.target.value) // 新しい length
              })
            }
          />
          12
        </label>

        <label>
          <input
            type="radio"
            value="24"
            name="length"
            checked={passwordData.length === 24}
            onChange={(event) =>
              setPasswordData({
                ...passwordData,
                length: parseInt(event.target.value)
              })
            }
          />
          24
        </label>
      </label>

      <br />

      {/* 数字を含めるかどうか（チェックボックス） */}
      <label>
        数字を含める：
        <input
          type="checkbox"
          checked={passwordData.hasNumbers}
          onChange={(event) =>
            setPasswordData({
              ...passwordData,
              hasNumbers: event.target.checked // true/false を受け取る
            })
          }
        />
      </label>

      <br />

      {/* 記号を含めるかどうか（チェックボックス） */}
      <label>
        記号を含める：
        <input
          type="checkbox"
          checked={passwordData.hasSymbols}
          onChange={(event) =>
            setPasswordData({
              ...passwordData,
              hasSymbols: event.target.checked
            })
          }
        />
      </label>

      <br />

      {/* パスワード生成ボタン */}
      <button type="submit">生成</button>
    </form>
  )
}

// App を #root に描画する
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
## ポイント  
✔ generatePassword に設定項目をまとめて渡している  
`generatePassword({ length, hasNumbers, hasSymbols })`  
こうすることで 複数の設定を 1 つのオブジェクトで扱える  
→ コードがスッキリして拡張もしやすい。  

✔ パスワード生成はランダム選択を繰り返すだけ  
`chars[Math.floor(Math.random() * chars.length)]`    
- chars の中からランダムに 1 文字取り出す  
- 回数は length 分だけ繰り返す  
これでパスワードが完成。  

✔ 子コンポーネントから親の関数を呼ぶ  
`<PasswordForm generatePassword={generatePassword} />`   
React でとてもよく使うパターン  