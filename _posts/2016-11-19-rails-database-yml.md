---
layout: post
title: "database.yml管理方法"
date: 2016-11-19
---

## database.yml管理方法
問題：異なるメンバーで、ローカル環境でユーザが必ず一致するわけではない

## 調査
- [x] `config` gem で管理可能？
    - やはりだめ。`database.yml`は早いタイミングで読み取れるので、他のサードパーティ制の gem などまだロードされてないので、エラーになってしまう
- [x] `database.yml`やめて `database.yml.example` にする場合、herokuは大丈夫？
    - 駄目っぽい。`database.yml`の`production`節の設定が必要

## 結論
- Heroku使ってないなら、`database.yml.exmaple`運用で良さそう
- Herokuの場合は、まだ分からない
 
    
    

