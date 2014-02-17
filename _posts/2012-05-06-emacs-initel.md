---
layout: post
title: "Emacs init.elを整理"
description: ""
category: Emacs
tags: []
---
最初からいろいろなコードを勝手に入れていましたが、よく整理してなくてバラバラだったので、整理してみる

順番も大事らしいので、順番どおり書く

1. load-pathを追加する関数を定義
2. 表示関連設定
    - color-theme
    - font
    - スクロールバー・行列番号表示・ファイルサイズ表示・タイトルバーにファイル名表示
3. デフォルト動き変更
    - iswatch
    - OSのコピベとyarkを共通化
    - インデントにタブを使わない
    - バックアップ・自動保存ファイル場所を一時フォルダに
    - キー定義
        - `C-t`で`switch-to-buffer`
        - `C-o`で`find-file`
        - `C-h`で`backward-delete-char`
4. 各モード設定
    - Markdown-mode
    - cua-mode
    - TODO : rinari (整理してから動かないので、後でやり直す)
    - Ruby-mode
    - RSpec-mode
5. 自動補完
    - auto-complete
    - yasnippet
6. 文字コード関連
    - デフォルト環境を日本語に
    - 設定ファイルをutf-8に
    - OSによるファイル名称・エンコーディング設定
7. パッケージ管理
    - ELPA
    - auto-install

以上
