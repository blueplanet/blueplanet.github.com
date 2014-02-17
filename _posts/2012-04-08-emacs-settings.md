---
layout: post
title: "Emacs初期設定"
description: ""
category: Editor
tags: [ Emacs]
---

Emacsを勉強したいので、簡単な初期設定をメモ  
* バージョン：Emacs 23.4 by [Cocoa Emacs](http://emacsformacosx.com/)

{% highlight cl %}
;; load-path設定
(setq load-path
  (append
    (list
      (expand-file-name "~/.emacs.d/site-list/ruby")
    )
   load-path))

;; 行番号表示
(setq line-number-mode t)
(global-set-key[f9] 'linu-mode)
(global-linum-mode t)

;; ruby-electric 対応括弧やEndなどの補完
(require 'ruby-electric)
(add-hook 'ruby-mode-hook '(lambda () (ruby-electric-mode t)))
{% endhighlight %}

* 疑問
    * ruby-modeはデフォルトで動いているらしい？？　ので、特に設定してない
    * インデントは、Ctrl+J/Ctrl+Oで改行すると、インデントしてくれるが、普通の改行はインデントなし
* 課題
    * Railsについては、後で調べて設定する

---
この記事の一部は、githubのオンライン編集機能で書きました、意外に`*`などでリストを作る時に、インデントしてくれる。いいですね。
