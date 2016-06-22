---
layout: post
title: "gemの中各クラスを一々requireしないといけない？"
date: "2016-06-23 06:00:34 +0900"
---

## 問題
- `gem`を作るときクラスを分けていますが、それらを使う場合、必ず`require`しないいけない。`Rails`のような`autoload`または一括で`require`する方法はないの？

## 調査
- [x] `Rails`以外で使う、有名な`gem`を調べてみる
  - [thor](https://github.com/erikhuda/thor)
    - 直接に`require`もあり、`autoload`もある
- [x] 直接に検索してみる
  - `gem autoload`
    - [[ruby]requireとautoloadについて](http://d.hatena.ne.jp/saronpasu/20080227/1204097516)

## 横展開
- `require`と`load`
  - `require`は二重読み込まない
  - `load`は、必ず読み込む
  - よって、設定ファイルを際読み込む場合は`load`を使う
- `ActiveSupport::Autoload`
  - 命名規約に従えばファイル名を省略できることで拡張したもの
  - `autoload :Hoge, 'hoge'` -> `autoload :Hoge`

## まとめ
- `autoload`は、Rubyのメソッドであり、`autoload :Hoge, 'lib/hoge'`のように書くと、必要なタイミングでロードしてくれるだけ
  - `require`と`autoload`は、今回のケースに対しては同じ、つまり、使っているクラスであればどっちでも書かないといけない
- `Raisl`の場合、クラスがない場合自動で探してくれるのは、`const_messing`メソッドを上書きして、命名規約に従って探すに行くだけ
- `gem`の場合は、やはり`require`しないといけない

## 参考リンク
- [Railsに組み込むgemを作るためのTips](http://naoty.hatenablog.com/entry/2013/01/20/122707)
- [Kernal#autoload](http://docs.ruby-lang.org/ja/2.3.0/method/Kernel/m/autoload.html)
