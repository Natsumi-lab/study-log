---
layout: post
title: "React 練習問題"
date: 2025-12-10
categories: [React]  
permalink: /React-practice/
---
## Reactのコードを書いていて疑問に思ったところの解説  
```
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const userName = '太郎'
  const renderTime = () => {
    const today = new Date()
    const year = today.getFullYear()
    const month = today.getMonth() + 1
    const day = today.getDate()
    return `${year}年${month}月${day}日`
  }

  return (
    <>
      <p>{userName}さん、こんにちは</p>
      <p>{renderTime()}</p>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
疑問：  
{userName}　には()がつかないのに  
{renderTime()}　にはなぜ()がつくの？  

回答： 
{userName} はただの文字列 “値” を表示しているので () はいらない  
{renderTime()} は “関数を実行して結果を表示” したいので () が必要  
関数は () をつけないと実行されません  

この問題では、JavaScriptの**「値」と「関数」**の違いを理解が必要  

---------------------------------------------------------------------------
```
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const skills = ['HTML', 'CSS', 'JavaScript', 'React']
  const url = 'https://code-lesson.com'
  const placeholder = '検索'
  const className = 'text'

  const SkillList = () => (
    <div>
      <ul>
        {skills.map((skill) => (
          <li key={skill}>{skill}</li>
        ))}
      </ul>
    </div>
  )

  return (
    <>
      <SkillList />
      <a href={url}>Code Lesson</a>
      <input placeholder={placeholder} />
      <p className={className}>テキスト</p>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
疑問1：
{skills.map((skill) => (  
のmapがどんな処理をしているのかよく分からない。  

回答：map は JavaScript の配列メソッド  
skills =  ['HTML', 'CSS', 'JavaScript', 'React']  
map は、この 4 つを 1 個ずつ取り出して処理する  

1回目：skill = "HTML" → <li>HTML</li> を作る  
2回目：skill = "CSS" → <li>CSS</li> を作る  
3回目：skill = "JavaScript" → <li>JavaScript</li> を作る  
4回目：skill = "React" → <li>React</li> を作る  

疑問2：
<li key={skill}>{skill}</li>   
なぜ２回も{skill}を書いているか？  

回答：2つの使い道が全く違うから  
① 1つ目の {skill}（key={skill} の部分）  
key は「React がその要素を特定するための名前（ID）」　　
keyがないと React は「どれがどれ？」と判別できなくなる　　
② 2つ目の {skill}
単にテキスト表示部分。実際には以下の様に表示されます。  
HTML  
CSS  
JavaScript  
React  

---------------------------------------------------------------------------
```
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [submitting, setSubmitting] = useState(true)
  //ここまで

  return (
    <>
      <form>
        <div>
          <input placeholder="お名前" />
        </div>
        <div style={{ 'margin-top': '5px' }}>
          <textarea placeholder="ご用件" />
        </div>
        <div style={{ 'margin-top': '5px' }}>
          {submitting ? (
            <button disabled>送信中</button>
          ) : (
            {
            }(<button>送信</button>)
          )}
        </div>
      </form>
    </>
  )
}
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
疑問1：  
なぜ一番下にReactDOM.createRoot・・・と書かれているのか？  

回答：  
App を先に定義しないと、ReactDOM が表示できないから  
コードの流れ
１、ReactDOM が読み込まれる
２、App コンポーネントが定義される
３、その App を画面に表示する（＝ ReactDOM.createRoot が実行される）
という順番が自然なので、**createRoot は一番最後に書く** という習慣になっています。  

createRoot がある場所（最下部）は「アプリのスタート地点」  
ReactDOM.createRoot(document.getElementById('root')).render(<App />)

React アプリの“起動処理”:      
1, HTMLファイルの <div id="root"></div> を見つける  
2, その中に React を描画する「準備」をする（createRoot）
3, <App /> を実際に表示する（render）  

もし App より前でReactDOM.createRoot(...).render(<App />)を書くと、  
React は App が何かわからないので、エラーになります。  

```
つまりこういう構造

index.html（HTMLの枠）
  ↳ <div id="root"></div> ← React の本体を入れる“箱”

main.js / App.js（Reactのコード）
  ↳ ReactDOM.createRoot(...).render(<App />)
     → root の箱の中に App を描画
```
root という名前は「慣例」  
appもよく使われる。自分で決めてもよい。  

疑問2：  
コードの一番下にReactDOM.createRootとあるが、 これはページ上部にあるimport ReactDOM from 'react-dom' とセットで使うのか？  

回答： 
はい。import ReactDOM と組み合わせて使う（読み込み → 使用）  
正確には以下の関係がある。    
import はライブラリを読み込み  
createRoot はそのライブラリの関数を実行する  

------------------------------------------------------------------------
```
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [count, setCount] = useState(1)
  const [data, setData] = useState({
    name: '山田',
    content: '内容'
  })

  const resetCount = () => {
    setCount(0)
  }

  const changeName = () => {
    setData({ ...data, name: '田中' })
  }

  const incrementCount = () => {
    setCount((prevCount) => prevCount + 1)
  }

  return (
    <>
      <div>
        <p>{count}</p>
        <button onClick={resetCount}>countをリセット</button>
        <button onClick={incrementCount}>countに+1</button>

        <p>{data.name}</p>
        <p>{data.content}</p>
        <button onClick={changeName}>data.nameを変更する</button>
      </div>
    </>
  )
}
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
---------------------------------------------------------------
```
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [message, setMessage] = useState('')
  return (
    <>
      <h2>プログラミングへの意気込みを書いてみましょう</h2>
      <input
        placeholder="意気込み"
        value={message}
        onChange={(e) => setMessage(e.target.value)}
      />
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
-------------------------------------------------------------------
```
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [visible, setVisible] = useState(true)
  const handleClick = () => {
    setVisible((prevState) => !prevState)
  }
  return (
    <>
      <div className="App">
        <button onClick={handleClick}>{visible ? '非表示' : '表示'}</button>
        {visible && <h2>ボタンを押してみましょう</h2>}
      </div>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
--------------------------------------------------------------------
```
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const buttonStyle = {
    borderRadius: '6px',
    border: '1px solid',
    padding: '8px 16px',
    cursor: 'pointer'
  }

  return (
    <>
      <h1 style={{ color: 'red' }}>色が変更できます</h1>
      <h2 style={{ backgroundColor: 'red', borderRadius: '24px' }}>
        背景色が変更できます
      </h2>
      <div>
        <button style={buttonStyle}>ボタン</button>
      </div>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
疑問：  
<h1 style={{ color: 'red' }}>色が変更できます</h1>　→{{}}二重になっている  
<button style={buttonStyle}>ボタン</button>　　　　→{}一つのみ  
このカッコの一重だったり二重だったりする理由は？  

回答：  
✔ 一重 { } → JSX 内で “JavaScript を使いますよ” の合図  
✔ 二重 {{ }} →　JavaScript の中で オブジェクト（{}）を渡している
ので、結果的に {} + {} の二重になっているだけ  

🔵 外側の { }  
→ JSX の中で JavaScript を実行しますよ これは React のルール）  

🔵 内側の { color: 'red' }  
→ JavaScript のオブジェクト（style オブジェクト）  

この場合、
```
const buttonStyle = {
  borderRadius: '6px',
  border: '1px solid',
  padding: '8px 16px',
  cursor: 'pointer'
}
すでに JavaScript のオブジェクトとして定義済み なので
style={buttonStyle}
と 変数を渡すだけ でOK。（外側の {} だけになる）
```
-------------------------------------------------------------
```
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const isHeading = true
  const isError = true

  return (
    <>
      <p style={{ fontSize: isHeading ? '30px' : '20px' }}>
        条件に応じてフォントサイズが変更できます
      </p>
      <span style={isError ? { color: 'red' } : { display: 'none' }}>
        エラー
      </span>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
疑問：  
これも｛｛と｛の違いを知りたい


--------------------------------------------------------------------
```
import React, { useEffect, useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [count, setCount] = useState(1)

  const incrementCount = () => {
    setCount((prevCount) => prevCount + 1)
  }

  useEffect(() => {
    console.log('useEffectを実行しました')
  }, [])

  useEffect(() => {
    console.log(count)
  }, [count])

  return (
    <>
      <div>
        <p>{count}</p>
        <button onClick={incrementCount}>+1</button>
      </div>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
-------------------------------------------------------------------------
```
import React, { useEffect, useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [count, setCount] = useState(1)

  const incrementCount = () => {
    setCount((prevCount) => prevCount + 1)
    console.log('incrementCount内のcount:' + count)
  }

  useEffect(() => {
    console.log('useEffect内のcount:' + count)
  }, [count])

  return (
    <>
      <div>
        <p>{count}</p>
        <button onClick={incrementCount}>+1</button>
      </div>
    </>
  )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
--------------------------------------------------------------------------
