---
layout: post
title: ShippableCI + rails
date: "2015-09-07"
published: true
---


## 設定メモ
1. [www.shippable.com](www.shippable.com) にサインアップ
2. プロジェクトを追加し、設定したいリポジトリを選択する
3. shippable.ymlファイル追加

### shippable.ymlファイル


### 注意点
基本的には参考記事の内容のままですが、下記の点を注意

- $BRANCHを判定し、masterブランチの場合だけheroukのpushするようにしている
- `heroku git:remote`でherokuのリポジトリURLを追加すると何故か下記のエラーになった為、直接に`git remote add`にしました

```
runtime: failed to create new OS thread (have 6 already; errno=11)
fatal error: newosproc
```




## 参考記事
- [ShippableでRailsアプリをCI＆CD](http://qiita.com/na999ta/items/b89da87a3e6ebd95dde6#2-6)
- [ShippableでCI 完全マスター](http://qiita.com/anoworl/items/441f9d7ced5b2737b385)
- [Using Heroku Postgres with Ruby on Rails](http://docs.shippable.com/heroku/#using-heroku-postgres-with-ruby-on-rails)
