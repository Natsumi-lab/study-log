---
layout: post
title: "React 問合せフォーム"
date: 2025-12-07
categories: [React]  
permalink: /submitForm/
---
Reactで問合せフォームを実装します。  
```
// 画面にReactコンポーネントを描画するためのライブラリ
import ReactDOM from 'react-dom'
// React本体と、状態管理に使うuseStateフックを読み込む
import React, { useState } from 'react'

// Appコンポーネント（問い合わせフォーム全体）
const App = () => {
  // 入力された名前を保持するstate（初期値は空文字）
  const [customerName, setCustomerName] = useState('')
  // メールアドレス
  const [email, setEmail] = useState('')
  // メッセージ内容
  const [message, setMessage] = useState('')
  // 名前が未入力かどうかのフラグ
  const [isEmptyName, setIsEmptyName] = useState(false)

  // フォーム送信時に呼ばれる関数
  const handleSubmit = (e) => {
    e.preventDefault() // ページがリロードされる標準動作を止める

    // 名前が入力されているか判定
    if (customerName !== '') {
      // 入力があればアラートメッセージを表示
      alert(`${customerName}さん、メッセージが送信されました！`)
    } else {
      // 名前が空ならエラーメッセージを表示するためのフラグをON
      setIsEmptyName(true)
      return // ここで処理を止める
    }

    // フォーム送信後、入力欄を空にリセット
    setCustomerName('')
    setEmail('')
    setMessage('')
  }

  return (
    <>
      {/* フォームのタイトル */}
      <h1> お問合せフォーム </h1>

      {/* フォームタグ。送信時にhandleSubmitが実行される */}
      <form onSubmit={handleSubmit}>
        <div
          style={{
            display: 'flex', // 要素をflexレイアウトにする
            flexDirection: 'column', // 縦方向に並べる
            marginBottom: '16px'
          }}
        >
          {/* 名前入力欄のラベル */}
          <label style={{ display: 'block' }} htmlFor="name">
            お名前
          </label>

          {/* 名前入力欄 */}
          <input
            style={{ width: '10em', border: '1px solid #333' }}
            type="text"
            id="name"
            name="customer_name"
            // stateと入力欄を同期
            value={customerName}
            onChange={(e) => {
              setIsEmptyName(false) // エラー表示を消す
              setCustomerName(e.target.value) // 入力値をstateに反映
            }}
          />

          {/* 名前が未入力の時だけ表示されるエラーメッセージ */}
          {isEmptyName && (
            <span style={{ color: '#FF0000' }}>
              お名前を入力してください
            </span>
          )}

          {/* メールアドレスのラベル */}
          <label style={{ display: 'block' }} htmlFor="email">
            メールアドレス
          </label>

          {/* メールアドレス入力欄 */}
          <input
            style={{ width: '15em', border: '1px solid #333' }}
            type="email"
            id="email"
            name="email"
            value={email} // stateと同期
            onChange={(e) => setEmail(e.target.value)} // 入力をstateに保存
          />

          {/* メッセージのラベル */}
          <label style={{ display: 'block' }} htmlFor="message">
            メッセージ
          </label>

          {/* メッセージ入力欄（複数行） */}
          <textarea
            style={{ width: '20em', border: '1px solid #333' }}
            type="text"
            id="message"
            name="message"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
          />
        </div>

        {/* 送信ボタン */}
        <button style={{ width: '100px' }} type="submit">
          送信
        </button>
      </form>
    </>
  )
}

// AppコンポーネントをHTMLの#rootに描画
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
## ポイント  
React では入力欄を「状態（state）」と同期させる  
value={customerName}  
と  
onChange={(e) => setCustomerName(e.target.value)}  
をセットで使うことで、

✔ 入力値が React の state に保存される    
✔ 表示される値も state から取る  

という双方向の仕組みが成り立っています。  
これを Controlled Component（制御されたコンポーネント） と呼びます。  

------------------------------------

バリデーション（入力チェック）  
if (customerName !== '') {  
  
未入力だったら state でフラグを立てる。  
これでエラーメッセージの表示非表示が切り替わります。  


