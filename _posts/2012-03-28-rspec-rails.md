---
layout: post
title: "RSpec on Rails (4)"
date: 2012-03-28 22:28
comments: true
tags: 
- Ruby
- Rails
- RSpec
---
　[スはスペックのス 【第 2 回】 RSpec on Rails (コントローラとビュー編)](http://jp.rubyist.net/magazine/?0023-Rspec)記事の写経

* Test::UnitはModelのテストをユニットテストとし、Controllerのテストを機能テストとしていますが、RSpecのほうは、Model・Controller・View・Helperのテストはすべてユニットテストとみなす
* ハマったこと
    * `render_template("blogs")`を書くと、レッドのはずだが、自分の環境では普通にグリーンになってる
    * 試してみたが、`blogs`と`blogs/show`と`show`と３つが全部OK。原因は分からず

* 調べたい
    * FactoryGril
    * Spoke
    * 浜松Rails3は資料いろいろ有りそう

　今日は、Controllerのテストまでに終わった。Viewのテストは次回で
