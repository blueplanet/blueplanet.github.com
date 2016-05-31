---
layout: post
title: "capistranoでrailsアプリをデプロイするメモ"
date: "2016-05-30 05:23:24 +0900"
---

最近のプロジェクトでデプロイ周りもいろいろ触れるようになったので、勉強した内容のメモ

## 環境
- Rails 4.2.5
- Capistrano 3.2.1

## 基本概念
- 導入と設定は [Capistrano 3系でRails4.1のデプロイ[rbenv][rvm][ruby2.1] - 酒と泪とRubyとRailsと](http://morizyun.github.io/blog/capistrano3-rails-deploy-multi-rbenv/) を参考しました
- 想定と違う内容
  - `shared`配下のディレクトリは自動で作成してくれない
  - `linked_files`は自動でアップロードしてくれない
  - この２つの課題はみんなどうやっているかを知りたいですね。

## カスタムタスク
- 上記の課題のようなものは自分でタスクを定義しておけば、`cap production deploy:upload` みたいに実行できるようになる
- 注意点
  - 何かを実行したい時は、デプロイ先で実行したいのか、`cap production deploy`コマンドを実行するマシンで実行したいのかを意識しておく必要がある
  - デプロイ先のサーバで実行したいコマンドは、`on do ... end`のように`on`のブロックの中に書かないと行けない（当たり前かも知れないけど）
