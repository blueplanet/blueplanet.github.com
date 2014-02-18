---
layout: post
title: "sportでRSpec on Railsのテストを高速化"
date: 2012-03-29 23:08
comments: true
categories: 
- spork
- RSpec
- Rails
---
　RSpecを勉強中。RSpecのテストを実行する時、何となく遅い気がしちゃいます。高速化する方法がないかと調べると、[github spork](https://github.com/sporkrb/spork)が出ました。

### まずは導入前後の比較、爆速ですね

|     | user  | system | cpu | total  |
|-----|-------|--------|-----|--------|
|導入前| 4.32s | 0.54s  | 99% | 4.898s |
|導入後| 0.19s | 0.05s  | 31% | 0.788s |

### 使い方もシンプル


1. `Gemfile`に`gem spork`を追加
2. `spork --bootstrap`を実行して、必要なファイルを生成する
3. `spork`を実行して、サーバを起動する
4. モデルの修正などを都度反映する為、テストキャッシュを無効化
    * `config/environments/test.rb`の`config.cache_classes = true`を`false`に修正 
5. `.rspec`に`--drb`実行オプションを追加
6. 普通に`rspec spec/**/*.rb`を実行する　　

以上

