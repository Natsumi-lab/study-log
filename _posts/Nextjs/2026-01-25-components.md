---
layout: post
title: "_components/ と components/"
date: 2026-03-23 (updated)
categories: [Next.js]  
permalink: /components/
---
# _components/ と components/ の違いは何か？  

これは Next.jsの仕様ではなく、設計上の“慣習”の違い です。  

## components/（アンダーバーなし）
一般的な意味：
- アプリ全体で再利用されるコンポーネント  
- ルーティングとは無関係  
- design system / UI kit 的  

例：  
Button  
Modal  
Dialog  
FormInput  

## _components/（アンダーバーあり）  
意味合い（慣習）：  
- このルート（または機能）に閉じた部品  
- ルーティングツリーの「補助物」  
- page / layout を支える存在  
アンダーバーは  
「ルーティングとして扱う意図はありません」という  
人間向けのシグナル。  
