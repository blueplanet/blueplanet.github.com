---
layout: post
title: "いまさらの zeus + guard の導入"
date: 2013-04-18 12:34
comments: true
tags: Rails, zeus, guard
---
`guard-rspec`を使って、`spec`が保存される都度自動実行されるが、`spec`を増えると、やはり実行時間に気になるので、何とかしたい

## zeus を選択する理由
* `Spork`と比較
    * `spec-helper`ファイルを汚染しない
* `Commands`と比較
    * `rspec`対応
    * `destroy`対応
    * `Commands`は両方とも使えいないらしい
* `Spring`と比較
    * `destroy`対応
    * `zeus start`１つプロセスで、異なるセッション間で共有出来る
        * `Spring`はセッション毎に`Spring server`が走ってるらしい

## 導入方法
* `Gemfile`の`development`グループに追加
    * `gem "zeus"
* `Guard`と併用している場合、`Guardfile`を修正
    * `guard 'rspec'`に`zeus: true`オプションを追加
    * このオプションによって、`Guard`が`rspec`を実行する時に`bundle exec zeus rspec`になる
    * `guard-zeus`という`gem`もありますが、これを使うと、`Guard`起動すると、`zeus start`を実行してくれるが、`spec`実行される通度、`zeus start`の内容と`spec`実行の出力とバラバラになってしまうので、却下
* `Cucumber`使わないとか、の場合は、`zeus.json`を作成
    * やったのは
        * 'Cucumber'を削除
        * 'dbconsole'を削除
        * 'runner'を削除
    * ちなみに、`zeus init`を実行すると、ひな形が作成してくれる

```
{
  "command": "ruby -rubygems -rzeus/rails -eZeus.go",

  "plan": {
    "boot": {
      "default_bundle": {
        "development_environment": {
          "prerake": {"rake": []},
          "console": ["c"],
          "server": ["s"],
          "generate": ["g"],
          "destroy": ["d"]
        },
        "test_environment": {
          "test_helper": {"test": ["rspec", "testrb"]}
        }
      }
    }
  }
}
```

## 問題
* `spec`の中に、`shared_context`を使ってるせいかもしれませんが、`zeus rspec ...`を実行する都度、`...not found shareed_context...`エラーになってる
    * 対策として、`spec_helper`の`require rspec/autorun`行を削除するとエラーがなくなった
    * 正直に言うと、その行のそもそもの役割はよく分かってない。削除しても、`rspec`とかも正常に実行出来るし

## まとめ
* こう設定すると、修正を保存する都度、`spec`が素早く実行されて、すごく快適です
