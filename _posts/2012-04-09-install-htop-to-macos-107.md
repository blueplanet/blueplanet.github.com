---
layout: post
title: "Install htop to MacOS 10.7"
description: ""
category: Application
tags: [MacOS, htop]
---
"@__69__"


　さんのつぶやきで、`htop`というカッコイイアプリを知りました。早速MacOS 10.7にインストール

`brew info htop`で検索してみたら、[homebrew issue](https://github.com/mxcl/homebrew/issues/10355)の説明を見ると、既に公式リポジトリから削除され、`Homebrew-alt`でそうぞとのこと。

なるほど。じゃ、やっちゃおう！

`brew install --HEAD https://raw.github.com/adamv/homebrew-alt/master/unmaintained/htop.rb`の一発でOK。

ジャン〜
![image](https://lh5.googleusercontent.com/-ZbutrCWxabg/T4LgZ7gX1-I/AAAAAAAAAuA/b8nEZ8brAEs/s800/Screen%2520Shot%25202012-04-09%2520at%252022.11.37.png)