---
layout: post
title: "wercker v2でrailsのテスト・デプロイまで"
date: "2016-05-04 09:46:04 +0900"
---

基本的には、[Dockerベースになったwercker v2でRailsアプリをCIしてSlackに通知、herokuにデプロイする手順 - Qiita](http://qiita.com/mikage014/items/b89b1113b4e88009da18) の内容どおりです。

## 最終のwercker.yml

```
box: ruby:2.3.0

services:
  - id: postgres
    env:
      POSTGRES_PASSWORD: secret_password

build:
  steps:
    - install-packages:
        packages: nodejs

    - script:
      name: Gem Nokogiri install
      code: bundle config build.nokogiri --use-system-libraries

    - bundle-install

    - rails-database-yml:
      service: postgresql-docker

    - script:
      name: echo ruby information
      code: |
          echo "ruby version $(ruby --version) running"
          echo "from location $(which ruby)"
          echo -p "gem list: $(gem list)"

    - script:
        name: Set up db
        code: RAILS_ENV=test bundle exec rake db:schema:load

    - script:
        name: rspec
        code: bundle exec rspec

  after-steps:
    # notify to Slack
    - wantedly/pretty-slack-notify:
        webhook_url: $SLACK_WEBHOOK_URL
        channel: test

deploy:
  steps:
    - heroku-deploy:
        install-toolbelt: true
        key-name: HEROKU_SSH_KEY
    - script:
        name: Update database
        code: heroku run rake db:migrate --app $HEROKU_APP_NAME

  after-steps:
    - wantedly/pretty-slack-notify:
        webhook_url: $SLACK_WEBHOOK_URL
        channel: test
```

## ハマったこと
- nokogiriがインストールできない
  - [CI wercker v2の使い方 - katlez’s blog](http://katlez.hatenablog.com/entry/2016/02/07/143127) を参考して、下記の部分を追加しました。

  ```
  - script:
    name: Gem Nokogiri install
    code: bundle config build.nokogiri --use-system-libraries
  ```
- js runtimeがない
  - 使うdockerのコンテナと関係あると思いますが、一応nodejsをインストールすることで解決できた

  ```
  - install-packages:
      packages: nodejs
  ```
- heroku デプロイでssh keyがデプロイ都度addされてしまう
  - `key-name`を設定したのに、毎回生成されて、結構はまった。解決ポイントは、`key-name`の値を指定するとき、環境変数を取得しにいくと思うが、一応`$`を要らないほうが正しいです。

    ```
    - heroku-deploy:
        install-toolbelt: true
        key-name: HEROKU_SSH_KEY
    ```

  - 公式の説明はこちらです。 [wercker - getting started - Heroku](http://devcenter.wercker.com/quickstarts/deployment/heroku.html) の`Using SSH Keys`のセクションのNoteに書かれてあります。
    - **Note: You should not prefix the key-name property with a dollar sign ($) or post-fix it with _PRIVATE or _PUBLIC.**

## 課題
- `nodejs`のインストールはまだ毎回走ってるようです。これを解決するためには、dockerのコンテナを作ってみてみたい
