---
layout: post
title: "渋谷EdgeRails勉強会"
date: 2012-03-26 19:19
comments: true
categories: 
- Rails
- 勉強会
---

　今日[第２回 渋谷Edge Rails勉強会 @masuidrive氏によるRails3講義 × 株式会社ドリコム事例発表](http://atnd.org/events/25274)参加に行きました

## Rails周り @増井さん
### Rails 歴史
* Ver 1の頃 CoCやDRYなどの特徴で注目される
* Ver 2 色々改善されるが、まだ課題が残ってる
    * Merbはより設計が綺麗なので、合流してRails開発に注力
    * Railsに関する周辺サービスが出てきた　Heroku / New relic / Engine yard
        * new relic：いわゆるサーバ監視サービスが、Railsのどのメソッド、レイアウトの描画か、サーバ通信か、どこが遅いのかを分析できるサービス    
* Ver 3以降：モジュール化
* Rails：変化を進んで受け入れる
    * Merbの吸収も同じ考え

### Build for Rails
* コード書く
    * Scaffoldは使わない
    * プラグインを使うか・使わないか　はじめは使うか
* テストを書く
    * 最低でも Test/Codeは３ 実際の実装＋テストは２倍で見積るほうが
    * UnitTest vs RSpec ModelはUnitTestで、以外は自作でHTTP経由のツールを使う
* 非同期処理
* パフォーマンス
* 運用
    * Passenger / Unicorn
        * Passenger 数分間のシャットダウンの時間がある
        * Unicorn Down時間ない
* レベル別デプロイ
    ＊ 開発者のみ　部内　社内　αユーザ　βユーザ　全体　というレベル付けて公開できるように

### Rails 4.0
* 今年らしい
* Ruby 1.9以上
* RESTfulを更に強化

### 宣伝
* Ruby公式資格教科書
* Taimnuim

## Railsのはまりポイント
* クラスのインスタンス変数
    * @foo ||= bar **bar**はDBから取得した値だったら、最新のデータに反映されない可能性が
* JSONが壊れた
    * MySQLのTextは65536バイトまでなので
* index名の長さ制限
    * MySQLの制限は64バイトまで
* tinyintのカラムは、ActiveRecordでBoolとして使ってる、数値ではない

ほかのネタもいろいろありましたが、全然分かっていません。。。   