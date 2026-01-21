---
layout: post
title: "globals.cssとtailwind.configの使い分け"
date: 2026-01-21
categories: [CSS]  
permalink: /globalcss&tailwind/  
---
# globals.css / tailwind.config / layout.tsx の使い分け  

## globals.css（“CSSそのもの” を書く場所）  
Tailwindの外側のことをやる場所  
例：フォント読み込み、body背景色、::selection、スクロールバー、CSS変数（--bg 等）  
shadcn/uiの推奨構成だと、色はCSS変数（HSL）でglobals.cssに置くことが多い  
「サイト全体の雰囲気（背景・文字色・ベース）」を決めるのに向いてる  

## tailwind.config（“Tailwindの設計図”）  
Tailwindに「このクラスを生成して」と教える場所  
例：content（どのファイルを走査するか）、theme.extend（色・余白・影・フォントサイズ追加）、plugins  
色をここに直書きする運用もできるけど、shadcn/uiと混ぜるなら  
「CSS変数（globals.css）→ Tailwindは bg-background みたいに参照」 が相性良い  

## layout.tsx（“アプリの骨組み（HTML構造）”）  
各ページの外側に共通でかかるもの  
例：<html lang="ja">、<body className="...">、共通ヘッダー/サイドバー、<Providers />  
bodyのクラス（min-h-screen bg-background text-foreground）をここで入れるのが典型  
**“構造”**担当。見た目はclassで調整するけど、CSS定義は基本しない  

## まとめ：  
layout.tsx = 骨組み / globals.css = 全体の見た目の土台（変数・ベースCSS） /  