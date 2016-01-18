---
layout: post
title: 'MySQL string'
date: '2016-01-19'
tags: Rails, MySQL
---

## 問題
**MySQLは、デフォルトの文字列は大文字小文字を区別しない！！！**

恥ずかしながら、これは知らなかったので、ハマってしまいました。。。

```ruby
irb(main):003:0> User.create name: 'user1', email: 'user1@test.com'
   (0.2ms)  BEGIN
  SQL (0.3ms)  INSERT INTO `users` (`name`, `email`, `created_at`, `updated_at`) VALUES ('user1', 'user1@test.com', '2016-01-18 21:06:05', '2016-01-18 21:06:05')
   (1.0ms)  COMMIT
=> #<User id: 1, name: "user1", email: "user1@test.com", created_at: "2016-01-18 21:06:05", updated_at: "2016-01-18 21:06:05">
irb(main):005:0> User.find_by name: 'user1'
  User Load (0.3ms)  SELECT  `users`.* FROM `users` WHERE `users`.`name` = 'user1' LIMIT 1
=> #<User id: 1, name: "user1", email: "user1@test.com", created_at: "2016-01-18 21:06:05", updated_at: "2016-01-18 21:06:05">
irb(main):006:0> User.find_by name: 'USER1'
  User Load (0.3ms)  SELECT  `users`.* FROM `users` WHERE `users`.`name` = 'USER1' LIMIT 1
=> #<User id: 1, name: "user1", email: "user1@test.com", created_at: "2016-01-18 21:06:05", updated_at: "2016-01-18 21:06:05">
```

## 対応方法

### 新規アプリの場合
- `db:create`を実行する前に、`config/database.yml`に`collation: utf8_bin`オプションを追加してから実行すること
- どうやってこの件を思い出せるのかな。。。

### 既存DBの新規テーブルの場合
- `ALTER DATABASE dbname DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;`
- **上記SQLを実行した以降のテーブルだけ役に立つ** DB全体の設定の変更なので、当然と言えば当然

### 既存のテーブルの場合（今回はこれ）
- `ALTER TABLE users MODIFIED name varchar(255) BINARY;`

## 参考記事
- [Rails の database.yml で collation を指定する](http://d.hatena.ne.jp/spitfire_tree/20140825/1408953117)
- [MySQLのCollationを理解するためにまとめてみた。](http://blog.6vox.com/2014/05/mysqlcollatoin.html)
- [Rails の Migration で MySQL の文字列型に BINARY 属性をつけるには - happy lie, happy life](http://d.hatena.ne.jp/spitfire_tree/20120627/1340789986)
