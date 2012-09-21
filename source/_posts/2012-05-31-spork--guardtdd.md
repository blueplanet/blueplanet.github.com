---
layout: post
title: "Spork + GuardによるTDD"
description: ""
category: Rails
tags: [Spork, Guard]
---
[Yokohamarb #20](http://bukt.org/events/31)に参加して、Railsの開発環境のお話しがありました。その中に、TDDツールについて、SporkとGuardが使われています。

Sporkはテストの事前ロードによりテストを高速化してくれるツールであるのは分かっていますが、Guardはわかっていなかったので、調べてみた

## Guard ##
- `Gemfile`を編集し、必要なgemを追加
    - `gem guard-spork`
    - `gem guard-rspec`
- `bundle install`でインストール
- `bundle exec guard init spork`
- `bundle exec guard init rspec`
- `bundle exec guard`でプロセス起動すれば、`spork`＋`guard`が起動される

これでテストの高速化かつ自動テスト環境が備えた
