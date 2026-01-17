---
layout: post
title: "モジュール"
date: 2026-01-16
categories: [Common]  
permalink: /githubActions/  
---
# github Actionのまとめ  

ymlファイルを作成します。    
```
プロジェクト/
 ├── .github/
 │    └── workflows/
 │         └── test.yml
 ├── src/
 ├── package.json
 └── ...
```
## CI とは？  
CI = Continuous Integration（継続的インテグレーション）  
一言で言うと  
コードを追加するたびに、自動でテスト・チェックを回す仕組み  

### 自動でできること  
lint（書き方チェック）  
typecheck（型チェック）  
test（テスト）  
build（ビルドできるか）  

## PR とは？  
PR = Pull Request   

## CI green とは？  
green = 成功  
GitHub の画面では：  
🟥 赤 → CI 失敗  
🟢 緑 → CI 成功  
「CI green」とはテスト・チェックが全部通った状態  

```
【流れ】
feature-ブランチ
   ↓
Pull Request（PR）
   ↓
レビュー & CI 実行
   ↓
lint / typecheck / build が走る
   ↓
全部OK(=CI green 🟢)なら main に merge
```