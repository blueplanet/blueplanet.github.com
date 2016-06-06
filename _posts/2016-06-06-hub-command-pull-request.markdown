---
layout: post
title: "hubコマンドでpull-requestを出す"
date: "2016-06-06 20:32:56 +0900"
---

`hub` コマンドでPullRequestを作成するのを活用し、指定したIDのPivotalTrackerのストリーのタイトルを取得してPullRequestのタイトルと説明を自動で埋めたい。

やり方は2つあります。

- `hub pull-request` コマンドのオプションを使う
  - この辺はいくつの記事があるので、参考すればOK
    - [GitHub の Pull Request 発行をもっとスマートに - Qiita](http://qiita.com/kasaharu/items/3ead40e6838fbb44d4cd)
    - [[GitHub]githubに「pull request」を送れたりするhubコマンドを導入 by Mac | Coffee Breakにプログラミング備忘録](http://to-developer.com/blog/?p=1372)
  - このやり方の気にならないのは、一発でPRを作成してしまうので、最後にPRの内容を確認したいというのはできない
- `hub pull-request`が読み取るファイルに事前にほしい内容を書き込む
  - `hub pull-request`を実行すると、`.git/PULLREQ_EDITMSG`ファイルを読み取っています。なので、ほしい内容を事前のそのファイルに書き出したうえで`hub pull-request`コマンドを呼び出せば、その内容で設定したエディターで内容を編集できるので、PRを送る直前に内容を確認できる
