---
layout: post
title: "default_scopeでorderを使うとき、効かないケースがある"
date: "2016-05-26 06:27:29 +0900"
---

`default_scope`なので、いつでも効くと勝手に思いましたが、効かないケースもあります。

## 環境
- Ruby 2.3.0
- Rails 4.2.6

## 前提

```
class User < ActiveRecord::Base
  has_many :posts
end

class Post < ActiveRecord::Base
  belongs_to :user

  default_scope { where('display_no > 0').order(:display_no) }
end
```

## 効かないケースを再現する

```
u1 = User.create name: 'u1'
u1.posts.create title: 't1', display_no: 3
u1.posts.create title: 't2', display_no: 2
u1.posts.create title: 't3', display_no: 1
```

`includes + references`で'LEFT JOIN`になる場合は、`default_scope`で定義した`where`条件は残されていますが、**order条件がなくなった！！**

```
irb(main):003:0> u1 = User.includes(:posts).references(:posts).first
  SQL (0.1ms)  SELECT  DISTINCT "users"."id" FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" = "users"."id" AND (display_no > 0)  ORDER BY "users"."id" ASC LIMIT 1
  SQL (0.1ms)  SELECT "users"."id" AS t0_r0, "users"."name" AS t0_r1, "users"."created_at" AS t0_r2, "users"."updated_at" AS t0_r3, "posts"."id" AS t1_r0, "posts"."title" AS t1_r1, "posts"."user_id" AS t1_r2, "posts"."display_no" AS t1_r3, "posts"."created_at" AS t1_r4, "posts"."updated_at" AS t1_r5 FROM "users" LEFT OUTER JOIN "posts" ON "posts"."user_id" = "users"."id" AND (display_no > 0) WHERE "users"."id" IN (1)  ORDER BY "users"."id" ASC
=> #<User id: 1, name: "u1", created_at: "2016-05-24 00:56:41", updated_at: "2016-05-24 00:56:41">
irb(main):004:0> u1.posts
=> #<ActiveRecord::Associations::CollectionProxy [#<Post id: 1, title: "t1", user_id: 1, display_no: 3, created_at: "2016-05-24 00:57:50", updated_at: "2016-05-24 00:57:50">, #<Post id: 2, title: "t2", user_id: 1, display_no: 2, created_at: "2016-05-24 00:57:54", updated_at: "2016-05-24 00:57:54">, #<Post id: 3, title: "t3", user_id: 1, display_no: 1, created_at: "2016-05-24 00:57:58", updated_at: "2016-05-24 00:57:58">]>
irb(main):005:0>
```

## おわりに
- 後で`default_scope`のソースを読んでみる
