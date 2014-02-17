---
layout: post
title: "GithubでOctopressを構築"
date: 2011-12-19 23:06
comments: true
categories: [Blog, Github, Octopress]
---

## きっかけ
* ブログを書きたい
* 現在ははてな日記を使ってる
    * もっとシンプルにしたい
    * Markdown記法とはてな記法を両方覚えるのはイヤ
    * バージョン管理したい
## はまったこと
* bundle installでエラー発生
    * ruby 1.9.2-p136だった
    * ruby 1.9.2-p280へバージョンアップしたら、通った
* rake setup_github_pagesでエラー発生
    * URL指定方が間違った。git@github.com:yourname/yourname.github.com.gitのような形式にすればOK

---
とりあえずは以上
