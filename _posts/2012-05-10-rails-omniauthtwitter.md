---
layout: post
title: "Rails OmniAuthでTwitter認証 写経"
description: ""
category: OmniAuth
tags: [Rails, Twitter]
---
下記の記事を参考して、写経してみた

[【Rails】OmniAuthを使ってtwitter / facebookで認証する](http://npb.somewhatgood.com/blog/archives/715)

- `rails new rails_oa`でアプリを作成
- ログインリンクでtwitterに遷移出来たが、callbackはまだうまく出来てない

## 漏れたところ ##
- Gemfileに追加した時、'gem pg'ではなく、pgだけ書いた
- route.rbのログアウトを定義してなかった
- `heroku rake db:migrate`を忘れた

一応ログイン・ログアウトは出来た
