---
layout: post
title: "自前でActionView::Baseを使ってrenderする時の罠"
date: "2016-01-26"
tags: Rails
---

ポイントを先に言いますと、**`action_view.render`の戻り値は文字列ではない、尚且つ、`to_s`しても文字列にならないこと** です。

## 前提
- `Job`処理の中、HTMLを生成したいので、`ActionView`を直接に使う

```ruby
def notification_body
  users = ...
  action_view = ActionView::Base.new(Rails.root.join('app', 'views'))
  action_view.assign users: users
  action_view.render(template: 'users/published.text.erb')
end
```

## 問題
- 外部APIを呼び出す為`notification_body`をエスケープしようとするタイミングで、よく分からない下記のエラーになってしまった

```ruby
NoMethodError: undefined method `each_byte' for nil:NilClass
from /Users/blueplanet/.rbenv/versions/2.1.5/lib/ruby/2.1.0/uri/common.rb:307:in `block in escape'
```

## 原因
- 最初に書いたんですが、`ActionView.render(...)`の戻り値は文字列ではないから`escape`がエラーになった

```ruby
[23] pry(main)> result = action_view.render(template: 'users/published.text.erb')
...
[24] pry(main)> result.class
=> ActionView::OutputBuffer
[25] pry(main)> result.to_s.class
=> ActionView::OutputBuffer
[26] pry(main)>
```

- さらに、**`result.to_s`しても文字列にならない！！！**

## 解決
- `to_str`メソッドを使えば、文字列になります。

```ruby
[27] pry(main)> result.to_str.class
=> String
[28] pry(main)>
```

## 参考記事
- [render_to_string の罠 - サッカーカレンダー(beta)の日記](http://d.hatena.ne.jp/scalendar/20140806/1407322110)
- [ActionView を単体で使ってみる | そんなこと覚えてない](http://blog.eiel.info/blog/2014/07/18/action-view/)

## 後で調査
- [ ] `ActionView::OutputBuffer`とは？
- [ ] `rake`の中で`mailer`を呼び出す際の処理は`ActionView`を使ってる？
