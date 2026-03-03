---
layout: post
title: "PostgreSQL SELECT"
date: 2026-03-03
categories: [SQL]  
permalink: /PostgreSQL-SELECT/
---
## SELECT  
SELECT は データを取得する命令です。  
```
SELECT カラム名
FROM テーブル名;
```

## 全件取得  
```
SELECT * FROM users;
```

* → すべてのカラム  
FROM → どのテーブルから取るか  

## 特定カラムだけ取得  
```
SELECT name, age FROM users;
```
必要な列だけ取得するのが基本。  

##  WHERE句（条件指定）  
```
SELECT * FROM users
WHERE age >= 20;


比較演算子（SQL共通）
演算子	意味
=	等しい
!= / <>	等しくない
>	より大きい
<	より小さい
>=	以上
<=	以下
BETWEEN	範囲
IN	含まれる
LIKE	あいまい検索
```

##  PostgreSQL特有のポイント  

### ① 大文字小文字の扱い  
- 標準SQL
LIKE は多くのDBで大文字小文字を区別する  

- PostgreSQL
LIKE → 区別する  
ILIKE → 区別しない（PostgreSQL特有）  
```
SELECT * FROM users
WHERE name ILIKE '%tanaka%';
```
👉 ILIKE はPostgreSQL独自拡張  

###  ② RETURNING句  
PostgreSQLでは SELECT だけでなく  
INSERT / UPDATE / DELETE の後に RETURNING が使える。  
```
INSERT INTO users (name, age)
VALUES ('田中', 30)
RETURNING *;
```
👉 追加したデータをそのまま取得できる  
（Supabaseでよく使う）  

### ③ 型キャスト（::）  

PostgreSQLでは :: を使う。  
```
SELECT age::TEXT FROM users;
```

標準SQLでは：  
```
SELECT CAST(age AS TEXT) FROM users;
```
👉 :: はPostgreSQL記法  

### ④ JSON操作が強い  
PostgreSQLはJSON型をネイティブサポート。
```
SELECT data->>'name'
FROM users;
```
👉 ->> はPostgreSQL独自  

### ⑥ ORDER BY（並び替え）  
```
SELECT * FROM users
ORDER BY age DESC;

ASC → 昇順（デフォルト）
DESC → 降順
```

### ⑦ LIMIT（取得件数制限）　　
```
SELECT * FROM users
LIMIT 10;
```
👉 上位10件だけ取得　　

MySQLも同じ書き方。　　
SQL Serverは TOP を使う。　　

### ⑧ GROUP BY（集計）  
```
SELECT age, COUNT(*)
FROM users
GROUP BY age;
```
👉 年齢ごとの人数を取得

PostgreSQLは標準SQLに忠実なので基本同じ。

### ⑨ 実務でよく使うSELECTテンプレート  
```
SELECT カラム
FROM テーブル
JOIN テーブル
ON 条件
WHERE 条件
GROUP BY カラム
ORDER BY カラム
LIMIT 数;
```


##  SQLとPostgreSQLの違いまとめ  
項目	標準SQL	PostgreSQL
LIKE	大小区別あり	同じ
ILIKE	なし	あり
型変換	CAST()	:: が使える
JSON操作	DB依存	強力に標準装備
RETURNING	DB依存	標準的に使える

## まとめ  
- SELECT はデータ取得の基本命令
- 文法はほぼ共通
- PostgreSQLは標準SQLに忠実
- ただし ILIKE, ::, RETURNING, JSON操作 が特徴