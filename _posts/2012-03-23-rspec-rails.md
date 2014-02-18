---
layout: post
title: "RSpec on Rails (3)"
date: 2012-03-23 22:36
comments: true
categories: 
- Ruby
- Rails
- RSpec
---

　RSpec on Railsｎの写経です。

　`lambda`と`change`マッチャは面白い、というか、まだよく理解出来てない

* 値の変化を評価する場合使う
* メソッドチェインなし：とにかく何かが変わった
* by(value)：１個増えた
* from(old).to(new)
* to(new)
* `change`にブロックを渡す場合、`do .. end`はサポートされない

成果物はこれ。

![image](https://lh4.googleusercontent.com/-j_NuxfNsDZY/T2yML-Z4QAI/AAAAAAAAAgc/z5iYAqnAL90/s800/120323-0001.png)

---
### 追記
Viを使ってるので、Unite.vimの設定をちょっと見なおした。正直に言うと、今までは、Uniteはあんまり使ってなかった、すみません。どんどん写経すると、やはりバッファだけではなく、カレントフォルダのファイル一覧も欲しくなるので、修正しました。

`noremap <C-P> :Unite buffer file<CR>`
