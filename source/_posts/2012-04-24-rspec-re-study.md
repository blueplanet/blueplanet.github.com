---
layout: post
title: "「改めて学ぶRSpec by Rubyist Magazine 0037号」を読んだ"
description: ""
category: RSpec
tags: [ Test ]
---
RSpecを勉強したいと思って、調べてみたら、素晴らしい記事を見つけた。

[ 改めて学ぶRSpec ](http://jp.rubyist.net/magazine/?0035-RSpecInPractice)

### xUnitっぽいRSpecコードの特徴 ###
* テストメソッドはすべてフラットで定義される
* `it []`＝＝test○○メソッド
* `before`==`setup`

### RSpecの流儀 ###
* `describe`はネストできるので、テストコードに構造が生まれるぞ！
* `context`は`describe`のエイリアスなんでが、`describe`==テスト対象、`context`＝＝テストする時の状況
* メリット：段階的に仕様をテストコードに落とし込む

{% highlight ruby %}
describe クラス do
  describe メソッド do
    context 正常値 do
      it {}
    end
    
    context 異常値 do
      it {}
    end
  end
end
{% endhighlight ruby%}

的なイメージ

* subject
    * subjectを使うことで、`should`のレシーバを省略できる
    * メリット：テスト対象が明確になる、ほぼ強制的に一つのテストケースに一つのアサーション歯科かけなくなる
    * `describe`のネストと被ってる場合は、新たに`describe`をネストさせてもう一度subjectを定義する

### itに文字列を渡すかどうか ###
* `it`に文字列を渡すと、コメントとコードの両方があるのは`DRY`ではないので、渡さないほうがいい
* デメリット：`--fs`オプションで実行する時の出力は日本語にならなくので、読みづらくなる
    * 解決：`context`にテスト内容を細かく書く
    * まあ、テスト結果が一番重要なので、実際はそこまでもやってない

### letでデータ以外のぶぶんだけを共通化 ###
* ブロックの評価値
* 値をシンボルと同名の変数に格納する
* テストケース毎に結果がキャッシュされる

これはまだ理解できてない気がする

### shared_context ###
* メソッドの間に共通的なcontextがある場合がある
* `shared_context`と`include_context`で共通化できる

* * * * *
まだまだRubyもRSpecも初心者なので、分からないところが多いところ
