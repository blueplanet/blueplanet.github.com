---
layout: post
title: "EmacsにYasnippet導入"
description: ""
category: Emacs
tags: [ Yasnippet ]
---
角谷さんのビデオでもYasnippetを使っていたらしいし、スニペットの展開が結構使われいるので、導入してみた

もともとGoogle codeで管理していたのですが、今の時点では、既にgithubに移行したので、インストールは基本 [README](https://github.com/capitaomorte/yasnippet)を参照すればOK。

## インストール ##
* `mkdir ~/.emacs.d/plugins`
* `cd ~/.emacs.d/plugins`
* `.emacs.d`は既にgitで管理しているので、submoduleとして追加する
`git submodule add https://github.com/capitaomorte/yasnippet .emacs.d/plugins/yasnippet`
* 設定ファイルに追加
`require 'yasnippet)`
`(yas/global-mode t)`

これでOK。

試しに、`Ruby-mode`で`mm`入力し`TAB`キーを入力すると、`method_missing`メソッドが展開された！！！

## RSpec スニペット ##

有志によりYASnippetの日本語ドキュメントがあるので、それ参照して、自分のスニペットを作れます

[Yet Another Snippet extension 日本語訳](http://yasnippet-doc-jp.googlecode.com/svn/trunk/doc-jp/index.html)

らしいですが、ファイルを作って、`yas`ロードパスに追加しても、まだ展開出来ていません。後でまた調べる
