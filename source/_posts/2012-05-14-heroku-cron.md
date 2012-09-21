---
layout: post
title: "HerokuでのCronについて調べてみた"
description: ""
category: Heroku
tags: [Cron]
---
Herokuの場合は、無料のCronは１日１回しか使えないイメージがある。現状はどうなっているかを調べてみた

## 現状 ##
- Cronは既に古い
- Heroku SchedulerはCronの代替案としてのものらしく、時間毎・１０分間隔でタスクを実行できる。
    - [http://blogjp.sforce.com/2011/11/heroku-schedule-dc69.html]()

- clockworkは秒単位まで指定できる
    - [http://d.hatena.ne.jp/marutanm/20110719/p1]()

## 料金 ##
- Heroku Schedulerは、プロセスが起動され、実行中はDynoの時間が計上される。例え、10:00起動され、10:06まで実行完了の場合は、Dynoの時間は0.1となるイメージ。
    - 最初は料金の計上を理解出来てなくて、`one-off admin process`で調べてみたら、下記の記事のお陰で、理解出来ました
        - [http://d.hatena.ne.jp/koshigoeb/20111112/1321070001]()
- clockworkは、裏側でプロセスが動いているので、ずっとDynoの時間が計上される

## 使い分け ##
- [http://d.hatena.ne.jp/ToMmY/20111121/1321802014]() を参考に
    - 他のアプリケーションの補佐として、軽い処理の場合は、Scheduler
    - 頻繁の処理（１０分以下）、または、ガッツリしたい処理は、clockwork
    - Schedulerは毎回プロセスが再起動される為、オーバーヘッドが大きい

- 自分なりの予想
    1. １つサービスでRailsアプリを運用し、DBは別にする
    2. もう１つサービスでclockworkを運用し、１のDBにアクセスする
