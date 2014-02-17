---
layout: post
title: "Node.js+express+MongoDBを試してみた"
description: ""
category: Node.js
tags: [Web, JS]
---
昨日の引き続き、MongoDBの部分も試してみた

## MongoDBインストール ##
- `brew install mongodb`
- `mkdir ~/Library/MongoDB_Data`
- `mongod --dbpath=/Users/XXX/Library/MongoDB_Data`
- `mongo`で接続出来た！

## コーディング ##
- `app.js`ルート追加
- `routes/index.js`にルート処理追加
- `views/mongo.jade`ビューを作成

出来たもの
![](https://lh4.googleusercontent.com/--5WdPuH5Jtw/T6mTtKV-hRI/AAAAAAAABKw/1_35PV___pA/s800/Screen%2520Shot%25202012-05-09%2520at%25206.43.30.png)

## 参考URL ##
- [NodeでMongoDBを使って連絡帳アプリを作る](http://codedehitokoto.blogspot.jp/2012/01/nodemongodb.html)
