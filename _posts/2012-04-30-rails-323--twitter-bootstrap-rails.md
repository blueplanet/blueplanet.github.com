---
layout: post
title: "Rails 3.2.3 + Twitter-bootstrap-railsを試して見る"
description: ""
category: 
tags: []
---
Rails勉強中。デザインが出来ないので、話題の`twitter-bootstrap`を試してみる

## 導入 ##
- `Gemfile`に`gem 'twitter-bootstrap-rails`を追加し、`bundle install`を実行
- `rails g bootstrap:install`を実行したら、エラーで落ちた
    - [twitter-bootstrap-railsのIssue](https://github.com/seyhunak/twitter-bootstrap-rails/issues/194)を参考して、`gem 'therubyracer', :platform => :ruby`をコメントを削除すると、正常に実行出来た
- 上記以外は、普通にReadmeに従ってやればOK

![](https://lh4.googleusercontent.com/-fuLcAMkDJUE/T56eWDMlpVI/AAAAAAAAA94/4suBZAEnSE4/s800/Screen%2520Shot%25202012-04-30%2520at%252023.14.04.png)
