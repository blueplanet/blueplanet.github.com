---
layout: post
title: "BPStudy#55 "
date: 2012-03-30 18:50
comments: true
categories: 
- Heroku
- 勉強会
---
今日ここの勉強会に行きました。適当なメモ  
[BPStudy#55 Herokuに関するお話](http://connpass.com/event/354/)

## Python本の説明
* プログラミングの話ではない
* 開発環境構築
* チーム開発
    * ドキュメント管理Sphinx
    * 課題管理
    * モジュール分散方法
    * CIツール
    * 複数サーバのデプロイツール
* 本番環境
    * Google AppEnige

##Heroku
* ホスティングサーバとの違い
    * OSなどには全く意識しないで製品に集中する
    * 生産性が高いプログラマが生産性をもっと高める為のプラットフォーム
    * Rubyとはある程度似ってる
* Salesforce？
    * 顧客管理システムを売る会社
    * では、なんでHerokuを買った？

###日本のHerokuは？
* Herokuを使いましょう！
* Facebookアプリを作りましょう！
* Herokuのアドオンを作りましょう！

##SonicGardenでのherokuの実践活用
* 会社
    * SKIP 社内NSS
    * youRoom
    * Ruby x Cloud x Ajile
    * 納品しない　受託開発
* 自社の運用方法
    * [高速で無駄のないソフトウェア開発を実現するための7つのポイント](http://kuranuki.sonicgarden.jp/2012/03/post-70.html)
        * [トラッカツール　pivotaltracker](http://www.pivotaltracker.com/)　**特徴：優先度ない、上からやれ！**
        * ドキュメントはほとんど作らない　**ホワイトボードで十分**
        * githubのソースコードレビューが素晴らしい
    * 新規サービスのスタートアップはHerokuを使ってる
    * ステージング環境としてHerokuを使う
* Herokuでの課題と工夫
    * ログを数年保管したい　**loggly addon**
    * 復旧できないとき
        * PG Backups addonを利用
        * DBをHerokuの外にバックアップ
        * heroku_backup_taskを拡張
* おすすめ資料
    * [Herokuで作るFacebookアプリ](http://gihyo.jp/dev/serial/01/heroku)
    * etc…

##デモ
ワンクリックでFacebookアプリ On Heroku のデモでした