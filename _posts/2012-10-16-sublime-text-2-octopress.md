---
layout: post
title: "sublime text 2 の octopress プラグインを作ってみました"
date: 2012-10-16 21:36
comments: true
tags: [ SublimeText2, Octopress]
---
## きっかけ
[Sublime Text 2] 購入してから1ヶ月ぐらいですが、ちょうど [Octopress] も使っているので、探してみたら、まだ専用のプラグインがないため、作ってみました。

<!-- more -->
## 実装周り
- 言語は好きな`ruby`ではなく`python`なんですが、これも勉強したい言語ではなるため、特に抵抗感がありません
- [Sublime Text 2] のプラグインに関するAPIのドキュメントは [公式サイト](http://www.sublimetext.com/docs/2/api_reference.html) 以外はほとんどなくて、あっちこっち探して、苦労していました
- 逆にデフォルトで入っている機能は、いろいろプラグインとして実装されるものが多い為、結構参考になりました。有難うございます！
- ハマったところ
	- 作ったものは、[Sublime Text 2]で直接に[Octopress]のコマンドを実行するので、[Sublime Text 2] 自体の制限により、[Sublime Text 2] をコマンドラインから起動しないと、環境変数はうまく引き継げないようです。これは結構ハマってました
	- 同期・非同期はいろいろ悩んだりしていました。最初は実行出来ればよくて、同期でいやと思ったが、結局、`rake generate`を実行すると、2,3秒程度ではない為、カーソルが回ってしまいます。気持ち悪いため、非同期にしました

## イメージ
![image](https://lh3.googleusercontent.com/-yFnkYy_h9bo/UHlZwhPHNKI/AAAAAAAACCE/njGTdOMnoD8/s800/Screen%2520Shot%25202012-10-13%2520at%252020.33.03.png)

## サポートコマンド

- `new_post`
- `new_page`
- `generate`
- `deploy`
- `gen_deploy`

## TODO

- `new_post`または`new_page`を実行した後は、自動でカーソルをタイトルのところを選択した状態にしたい
- カテゴリは全部手入力だと間違いやすいので、リストで選択できたらいいなと思う
- `preview`もしたいが、どこまでするかはまだ悩んでる

## URL
- PackageControlリポジトリに登録し、PullRequestを送りましたが、まだ承認をもらってないため、とりあえずは、直接ダウンロードだけです。

こちらへどうぞ [sublime-text-2-octopress](http://github.com/blueplanet/sublime-text-2-octopress)

[Sublime Text 2]: http://www.sublimetext.com/
[Octopress]: http://octopress.org/
