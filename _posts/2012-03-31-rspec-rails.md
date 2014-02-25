---
layout: post
title: "Rspec 2.9.0 on Rails 3.2.2 + sporkで試す"
date: 2012-03-31 23:21
comments: true
tags: 
- Ruby
- Rails
- RSpec
---
バージョンが異なる場合は、いろいろ違いがあるので、最新バージョンで環境を作って、最新のAPIを確認したい

###バージョン
* ruby 1.9.2
* rails 3.2.2
* rspec 2.9.0
* spork 0.9.0

##手順
* Gemfileに追加
    * `gem 'rspec-rails'`
    * `gem 'spork'`
* `bundle install`を実行してgemインストール
* `rails g rspec:install`でrspec関連ファイルを生成
* `spork --bootstrap`でspork関連ファイルを生成
* `rails g scaffold post title:string body:text`を実行すると、ファイルがいっぱい生成される
    * この中に`spec/view/`配下のファイルを参考してViewテストの書き方を勉強したい
    
* ハマったこと
    * `Gemfile`に追加する時、`gorup`を使いたかったが、`:development, :test`で限定すると、何か`rails g scaffold …`で生成する時、spec関連のテストクラスは作ってくれない
    * やはり`Gemfile`の書き方はよくわかってないせい。とりあえずは、直接に`group`は書かない
