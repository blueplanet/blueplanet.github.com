---
layout: post
title: "Rails開発の場合は vim と emacsとは、どっち？"
description: ""
category: Editor
tags: [ Rails, Vim Emacs ]
---

Rails開発を勉強中。Editorについての考え

* Textmate：No1なんですが、日本語や中国語の入力がおかしいのは痛いので断念
* Vim：最初から今まで使ってるが、さすがに設定をこなさないと
    * 使ってるもの `vim-rails`,`neocomplcache`,`unite`,`nerdtree`(不要かな？)  
      と言っても、入れているだけで全然こなしてないので、もっと調べないと
    * 括弧やクォーテーションなどの入力は、対になってるものが自動補完　←　vimrcでキーバインドで解決
    * `def`や`if`などをタイプする場合、`end`を自動挿入してもらいたいが、今まだ出来てない  
      調べてみたら、`vim-endwise`があるらしいが、使い方は今だに分かってない
* Emacs：角谷さんのRspecの実演を見ると、カッコよくてすごく刺激を受けましたので、勉強したい
    * これからなので、まだ評価できないレベル
    * Matzさんが使ってるので、Rubyの周りは完璧じゃないかなと期待してる
    * `Rinari`というプラグインがあるらしい

しばらくはVimを使って、Emacsも使ってみようと