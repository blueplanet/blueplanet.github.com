---
layout: post
title: ShippableCI + rails
date: "2015-09-07"
published: true
---


## 設定メモ
1. [www.shippable.com](www.shippable.com) にサインアップ
2. プロジェクトを追加し、設定したいリポジトリを選択する
3. shippable.ymlファイル追加

### shippable.ymlファイル

```ruby
language: ruby
rvm:
  - 2.2.3
cache: true
env:
  global:
    - CI_REPORTS=shippable/testresults COVERAGE_REPORTS=shippable/codecoverage
    - APP_NAME=sns-news
    - secure: xxxxx # プロジェクトの設定ページでHEROKU_API_KEY=xxxxの値を暗号化した値をここに設定する

before_install:
  - source ~/.rvm/scripts/rvm
  - rvm install $SHIPPABLE_RUBY --verify-downloads 1
  - source ~/.bashrc && ~/.rvm/scripts/rvm && rvm use $SHIPPABLE_RUBY
  - which heroku || wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

install:
  - bundle install --gemfile="Gemfile"
  - ruby -v

before_script:
  - cp config/database.yml.sample config/database.yml
  - bundle exec rake db:create

script:
 - bundle exec rspec

after_success:
  - if [ "$BRANCH" == "master" ]; then test -f ~/.ssh/id_rsa.heroku || ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.heroku && heroku keys:add ~/.ssh/id_rsa.heroku; fi
  - if [ "$BRANCH" == "master" ]; then git remote -v | grep ^heroku || git remote add heroku git@heroku.com:$APP_NAME.git; fi
  - if [ "$BRANCH" == "master" ]; then git push -f heroku master; heroku run rake db:migrate; fi
```

### 注意点
基本的には参考記事の内容のままですが、下記の点を注意

- $BRANCHを判定し、masterブランチの場合だけheroukのpushするようにしている
- `heroku git:remote`でherokuのリポジトリURLを追加すると何故か下記のエラーになった為、直接に`git remote add`にしました

```shell
runtime: failed to create new OS thread (have 6 already; errno=11)
fatal error: newosproc
```

## 参考記事
- [ShippableでRailsアプリをCI＆CD](http://qiita.com/na999ta/items/b89da87a3e6ebd95dde6#2-6)
- [ShippableでCI 完全マスター](http://qiita.com/anoworl/items/441f9d7ced5b2737b385)
- [Using Heroku Postgres with Ruby on Rails](http://docs.shippable.com/heroku/#using-heroku-postgres-with-ruby-on-rails)
