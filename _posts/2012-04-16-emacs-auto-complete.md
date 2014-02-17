---
layout: post
title: "Emacs Auto complete"
description: ""
category: Emacs
tags: []
---
EmacsでRSpecを書いていますが、Ruby-modeで括弧が補完してくれて、メソッドなども`end`が補完してくれるけど、入力が多くなると、やはり変数名などよく入力するキーワードが自動補完したいですね。

そこは、`Auto Complete Mode`の出番。

インストールは[公式サイト](http://cx4a.org/software/auto-complete/index.ja.html)からダウンロードしてから、解凍したフォルダの`/etc/install.el`を`M-x RET load-file`でロードする。その後、`init.el`に設定を追加すればOK

{% highlight cl %}
(require 'auto-complete-config)
(add-to-list 'ac-dictionary-directories "~/.emacs.d/elisp//ac-dict")
(ac-config-default)
{% endhighlight %}

* * * * *
しばらくは、３つのキーの設定を変更してみたい

{% highlight cl %}
(global-set-key[(control t)] 'switch-to-buffer)
(global-set-key[(control o)] 'find-file)
(global-set-key[(control h)] 'backward-delete-char)
{% endhighlight %}
