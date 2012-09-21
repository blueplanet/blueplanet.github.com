---
layout: post
title: "RSpec on Rails (2)"
date: 2012-03-21 06:31
comments: true
categories: 
- Ruby
- Rails
- RSpec
---

　引き続き

* before /after
    * setup / teardown に相当
    * 一度だけ実行したい場合、`:all`を指定する
    * 各example毎実行したい場合、`:each`を指定する **デフォルト**
    * 複数指定もできる。実行順番は
        * before系は定義順
        * after系は定義順の逆順

* should / should_notはRSpecによるObjectに対する拡張するメソッド
* マッチャ
    * 演算子マッチャ` < <= == === =~ > >=` **否定のものはサポートされない、should_notを使うべき**
    * `be_close change eql equal have(n).items have_at_least(n).items have_at_most(n).items have_exactly include match raise_error respond_to satisfy throw_symbol`
    * `be_XXX`　→　`XXX?`
    * `be_true / be_false`は`true? / false?`というメソッドがなくても使える
    * `have_XXX` →　`has_XXX?`
    * カスタムマッチャ定義できる
* `violated`は実行を失敗させる
* 保留：テストまだ書いてない
    * `it`にブロックを渡せない
    * pendingメソッドを使う

### ようやく `RSpec on Rails`
* 今更気づいたのですが、記事のバージョンは結構古い。よって、コマンドとかいろいろ変わっている
    * インストール：Gemfileに`gem 'rspec'``gem 'rspec-rails'`追記してから、`bundle install`実行すればOK
    * 初期化は`rails g rspec:install`
    * `rake spec`の補完一覧
    ![image](https://lh6.googleusercontent.com/-gpjoO4UhGbE/T2s9WOraeTI/AAAAAAAAAgI/FO8ATshJGwU/s800/120322-0004.png)
    * `TODO:`モックとスタブの区別はわからない
    * フォルダ構造もシンプルになっているよう。少なくともデフォルトは作ってない
    * `script/generate rspec_model`を実行する必要がなくなり、上記の`rails g rspec:install`を実行しておけば、普通に `rails g …`を実行する時、`test`系ではなく、`rspec`系のテストクラスを作ってくれる
    * ruby 1.9以降を使う+日本語がある場合、例の`encoding=utf-8`追記する必要
    
---
複数記事に分けるので、とりあえずここまで