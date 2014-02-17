---
layout: post
title: "RSpec-modeを試してみた"
description: ""
category: Editor
tags: [Emacs, RSpec]
---
Emacsの設定をちょっとずつ進めたいと思って、Yasnippetのカスタマイズスニペットを使いたいが、自分のスニペットのパスはどうやって設定できるかはイマイチわかってないため、出来なかった

ちょっと調べたら、`RSpec-mode`があるのをわかって、導入してみた。

[rspec-mode](https://github.com/pezra/rspec-mode)

## 状況 ##
- インストールができて、specファイルを開いたらちゃんとrspec-modeになる
- `c-c , v`でspecを実行するのはうまく出来てない

## 問題 ##
- この`rspec-mode`は、rails向けらしく、`Rakefile`によってプロジェクトのルートパスを確定していく
    - 実際試したのは、`spec/game_spec.rb`と`lib/game.rb`のような２つファイルしかない軽いものなので、`rails`ではないので、困った
    - `touch Rakefile`で空のファイルを作成してみると、実行できたっぽいが、内容はおかしい。実際はspecは走ってないらしい
    - 要するに、実際は失敗のケースを実行しても、「成功したよ」とのメッセージが来るわけ
    - [rspec-modeをrake意外で使う方法](http://d.hatena.ne.jp/nbahide/20100721/1279676604)を参考して、設定してみたら、今回は`zsh:rspec command not found`になった
- 調べてみたら、`zsh`と`rvm`とを併用する場合は、別の設定が必要らしい
    - [rspec-mode](https://github.com/pezra/rspec-mode)のReadmeに書かれていますが、`Zsh and RVM`だったら、また別の設定が必要
    - 設定してみたら、`bash :rspec command not found`になった
    - `rvm`の設定がおかしいかと、確認したら、問題なさそう
    
明日調べよう
