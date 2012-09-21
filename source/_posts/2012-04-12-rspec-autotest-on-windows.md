---
layout: post
title: "RSpec AutoTest on windows"
description: ""
category: 
tags: []
---
昨日Lionで環境を構築したが、今回はWindows上の構築手順

## 環境 ##

* Windows XP
* RSpec 4.9.0
* ZenTest 4.7.0

## 基本環境手順 ##

* `mkdir rspec`
* `cd rspec`
* `mkdir spec`
* `mkdir lib`
* RSpecの設定ファイルを作っておく

    * `echo --format nested > .rspec`
    * `echo --color >> .rspec`

* この時点で`autotest`を実行すれば、ファイルが保存される都度、テストが走るはず

## Growに通知 ##

* `gem install autotest-growl`
* [Growl for Window](http://www.growlforwindows.com) をインストール
* `echo require 'autotest/growl' > .autotest`
* `autotest`再起動

## WindowsのCMDでの色表示問題を解決 ##
`.rspec`ファイルに色表示オプションを追加している場合は、Windowsでは色コードがうまく認識できないので、表示がおかしくなる

* 解決方法は、こちらを参考してできました  
[Windows環境でRSpecなどの出力に色を付ける](http://lajvard.hatenablog.com/entry/2012/03/03/194359)

以上で構築完了

---
ちなみに、Growl for Windowsは、デュアルディスプレイの場合は、ちゃんと認識してくれるので、表示設定はどのディスプレイのどの部分に表示するのを設定できる。素晴らしいですね。
