---
layout: post
title: "heroku postgresql upgrade"
date: "2016-05-10 19:44:26 +0900"
---

無料のプランからhobby-basicにアップグレードするメモ

## 手順

**無料のプランがfllow機能が使えないので、一旦作成してからデータをコピーするしかない**

1. 新しいDB(hobby-basic)を作成

```
heroku addons:create heroku-postgresql:hobby-basic
```

2. メンテーモードにする

```
heroku maintenance:on
heroku ps:scale worker=0
```

3. データコピー

```
heroku pg:copy DATABASE_URL HEROKU_POSTGRESQL_COBALT_URL
```

4. データURL切り替え

```
heroku pg:promote HEROKU_POSTGRESQL_COBALT_URL
```

5. メンテーモードをオフ

```
heroku ps:scale worker=1
heroku maintenance:off
```

## 参考したリンク
- [herokuのpostgresqlのプランをアップグレードした | 自転車で通勤しましょ♪ブログ](http://319ring.net/blog/archives/2991/)
- [heroku / データベースのアップグレード | Workabroad.jp](http://www.workabroad.jp/posts/2073)
- [Upgrading Heroku Postgres Databases | Heroku Dev Center](https://devcenter.heroku.com/articles/upgrading-heroku-postgres-databases)
