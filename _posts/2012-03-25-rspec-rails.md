---
layout: post
title: "RSpec on Rails (4)"
date: 2012-03-25 09:08
comments: true
tags: 
- Ruby
- Rails
- RSpec
---
　写経の記録

* `before_save`メソッドを定義しても、呼び出されてないらしい。下記を参考して修正  
  [RSpec2.4.0をRails3.0.3で学ぶ(その1)](http://d.hatena.ne.jp/NowTom/20110129/1296271461)
  
* 疑問：テスト実行はちょっと遅い気がする。試しにTimeでトータル時間を測定してみたらこんな感じ
![image](https://lh5.googleusercontent.com/-KSxPT5UHQCY/T25luPLyRYI/AAAAAAAAAg0/ZJtVEwaUEvw/s800/120325-0001.png)
  userとsystemはどっちがどっちかはよく分からない。要するに、テストロジック自体は0.219かかったが、テストコマンドの全体は3.96かかったとは、間違いないと思いう。もちろん、Rails環境のロードも時間がかかるのはわかってるけど。

　Rails環境を事前にロードしておく方法があるようなので、後で調べます
　
---

ブログの画像をPicasawebを使ってますが、毎回Safariでアップロードし、URLを取得して文書に貼り付けとの流れだけど、面倒なので、よりいい方法があるかを調べる
