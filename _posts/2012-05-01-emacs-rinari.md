---
layout: post
title: "Emacs Rinariを導入して見る"
description: ""
category: Emacs
tags: [Rails]
---
Railsを勉強中。やはりEmcsで編集したいので、`Rinari`を導入してみる

## ELPAの試しと失敗 ##
- ELPAからのインストールがおすすめらいいので、まずはELPAを導入
- `auto-install`を使って`ELPA`をインストール(ややこしいな)
- 今までは、起動が遅くなる為、`auto-install-update-emacswiki-package-name`をコメントアウトしてるので、まずは復活する
- `auto-install-update-emacswiki-package-name`を実行
- `M-x install-elisep`でインストールを起動
- URLが聞かれるが、なんか`[http://bit.ly/pkg-el23](http://bit.ly/pkg-el23)`は`reject`されるので、直接に展開後の長いURLを入力して実行した
- `init.el`に必要な設定を追加

    ;; package.el
    (when (require 'package nil t)
      (add-to-list 'package-archives
                   '("marmalade" . "http://marmalade-repo.org/packages/"))
      (add-to-list 'package-archives '("ELPA" . "http://tromey.com/elpa/"))
      (package-initialize))

### Rinariをインストール ###
- ELPAは初回実行なので、`list-package`を実行する
- `Cannot open load file: ruby-compilatio`エラーで落ちた
- 原因はよく分からないので、とりあえずはやめた

## 直接インストール ##
- 基本は、[Basic-Setup](http://rinari.rubyforge.org/Basic-Setup.html#Basic-Setup)を参照する
- `submodule`を利用するので、
    - `git submodule add git://github.com/eschulte/rinari.git .emacs.d/elisp/rinari`を実行
- `rhtml-mode`は`rinari`のものを使う
    - `git submodule add https://github.com/eschulte/rhtml.git .emacs.d/elisp/rhtml`

* * * * *
これで、ビューファイルを開くと、`rhtml-mode`になって、`rinari`も有効になるはず


