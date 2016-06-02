---
layout: post
title: "capistrano-bundlerでデプロイしたときにbinの下に空っぽ"
date: "2016-06-02 23:54:58 +0900"
---

## 問題
ステージング環境を構築して`cap` でデプロイしてみたら、binのしたに何もないので、`rails c`などは実行できない

## 原因
[bundler/CHANGELOG.md at master · capistrano/bundler](https://github.com/capistrano/bundler/blob/master/CHANGELOG.md#114-22-jan-2015) で自動生成してくれないようになった

議論はこの辺 [Don't generate binstubs by default when using Rails 4? · Issue #45 · capistrano/bundler](https://github.com/capistrano/bundler/issues/45)

## 対応方法
上記の議論しているIssueにかかれてあるままですが、

- `deploy.rb`に`set :bundle_binstubs, nil`を追加する
- `linked_dirs`から`bin`を削除する

ことで、`bin`配下に普通に実行ファイルが出来ましtあ。
