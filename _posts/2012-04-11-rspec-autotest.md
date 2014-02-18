---
layout: post
title: "RSpec AutoTest"
description: ""
category: 
tags: []
---
角谷さんのビデオを見て、すっごく刺激を受けたので、自分もRSpecのAutoTest環境を構築したくなりました

## 環境 ##
* Mac OS 10.7.3
* Ruby 1.9.2
* RSpec 2.9.0
* ZenTest 4.7.0

## 1.AutoTest基本環境 ##
基本は公式のWikiを参考する

* `mkdir rspec2`
* `mkdir spec lib`
* `echo --format nested > .rspec`
* `echo --color >> .rspec`
* `autotest`を実行し、自動テストを起動する

以降は、普通に`spec`にspecファイルを書き、`lib`にロジックファイルを書いて、保存する都度テストが再実行される

すごく使いやすくなりましたね。前までは、`spec_helper`ファイルを作成とか、いろいろやらないといけなかったけど、もう`gem`さえインストールすれば、全部やってくれましたね!素晴らしい！

## 2.Growlに通知 ##

* `gem install autotest-growl`
* `~/.autotest`ファイルに`require 'autotest/growl`を追加
* `autotest`を再起動

これでファイル保存する都度、Growlでテスト実行結果のアイコンが表示される

よし!角谷さんのビデオを見ながら、写経しましょう！



