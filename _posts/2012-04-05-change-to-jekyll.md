---
layout: post
title: "OctopressからJekyllへ"
description: ""
category: Blog
tags: [Jekyll, Octopress]
---

いろいろ考えた結果、やはりJekyllのほうが使い勝手が好きで、移行するメモ

## 方針
今までの記録を保持したいので、githubのリポジトリは、クリアしなくて、できるだけ記録を保持するままで移行

## 手順
* `git pull origin master`
* `rm -rf *`
* `cp ../jekyll/* .`
* `git add .`
* `git ci -m 'jekyll init'`
* 以降は、jekyllの手順を従って設定するだけ

