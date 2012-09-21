---
layout: post
title: "Emacs daemonモード試してみる"
description: ""
category: Emacs 
tags: [Editor]
---
Lionで`Emacs`を使って、一番悩んでるのは、コマンドラインからファイルを編集するケース。前から`daemon`モードが知っていますが、何か`emacs --daemon`で起動すると、エラーが出て起動出来ない状態だった。

今日冷静に考えてみたら、GUI関連の設定とフォント設定の周りのせいじゃないかと気付きました。

確認したら、やはりフォントの設定は条件なしでやっているのは悪かった。それを`window-system`の場合のみ設定するように修正すれば、`daemon`オプションを付けても実行出来た！！

これで、Emacsには文句なしですね。

Daemonの設定方法について、公式のWikiに書かれています。

[Emacs As Daemon](http://www.emacswiki.org/emacs/EmacsAsDaemon)
