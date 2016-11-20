---
layout: post
title: "Heroku SSL証明書"
date: 2016-11-20
tags: [Rails Heroku SSL]
---

# Rails heroku SSL

1. add gem, add config
2. gene heroku token

    ```
    ╰─$ heroku authorizations:create -d "LetsEncrypt"
    Creating OAuth Authorization... done
    Client:      <none>
    ID:          xxxxxxxxx
    Description: LetsEncrypt
    Scope:       global
    Token:       xxxxxxx
    ```

3. run config

    ```
    heroku config:add ACME_DOMAIN=xxx.io,www.xxx.io ACME_EMAIL=master@xxx.io HEROKU_TOKEN=xxxxxxxxx HEROKU_APP=you_app
    ```

4. run renew

    ```
    heroku run rake letsencrypt:renew
    ```
    
SSLできた！    

## エラー
- エラー内容

    ```
    Testing filename works (to bring up app)...rake aborted!
    redirection forbidden: http://sns-news.herokuapp.com/.well-known/acme-challenge/pOR2y80x8Gtx7w7QRpAZPskGwK7v0pXa_LmSJF8SjTE -> https://sns-news.herokuapp.com/.well-known/acme-challenge/pOR2y80x8Gtx7w7QRpAZPskGwK7v0pXa_LmSJF8SjTE
    /app/vendor/bundle/ruby/2.3.0/gems/letsencrypt-rails-heroku-0.2.7/lib/tasks/letsencrypt.rake:55:in `block (3 levels) in <top (required)>'
    /app/vendor/bundle/ruby/2.3.0/gems/letsencrypt-rails-heroku-0.2.7/lib/tasks/letsencrypt.rake:32:in `each'
    /app/vendor/bundle/ruby/2.3.0/gems/letsencrypt-rails-heroku-0.2.7/lib/tasks/letsencrypt.rake:32:in `block (2 levels) in <top (required)>'
    /app/vendor/bundle/ruby/2.3.0/gems/bugsnag-5.0.1/lib/bugsnag/rake.rb:12:in `execute_with_bugsnag'
    /app/vendor/bundle/ruby/2.3.0/gems/rake-11.3.0/exe/rake:27:in `<top (required)>'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli/exec.rb:74:in `load'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli/exec.rb:74:in `kernel_load'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli/exec.rb:27:in `run'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli.rb:332:in `exec'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/vendor/thor/lib/thor/invocation.rb:126:in `invoke_command'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/vendor/thor/lib/thor.rb:359:in `dispatch'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli.rb:20:in `dispatch'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/vendor/thor/lib/thor/base.rb:440:in `start'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/cli.rb:11:in `start'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/exe/bundle:34:in `block in <top (required)>'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/lib/bundler/friendly_errors.rb:100:in `with_friendly_errors'
    /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.13.6/exe/bundle:26:in `<top (required)>'
    ```

    - 対応方法：SSL自体はまだ動かないので、一旦`force_ssl = false`にする


