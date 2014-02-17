---
layout: post
title: "rails3.2 on heroku"
date: 2012-04-02 07:29
comments: true
categories: [ Rails, Heroku ]
---

Rails 3.2のアプリをHerokuにデプロイする場合、ハマったことをメモ

##環境

* Rails 3.2.2
* ローカルDBはsqlite

##手順
* Railsアプリを作成
    * `rails new myblog`
    * `cd myblog`
    * `git init`
    * `git add .`
    * `git ci -m 'init'`
* Scaffold
    * `rails g scaffold post title:string body:text`
    * `rake db:migrate`
    * `rm public/index.html`
    * `vi config/route.rb`
        * `root :to => "pots#index"`を追加
    * `git rm public/index.html`
    * `git add .`
    * `git commit -m 'scaffold'`
* Heroku
    * `vi Gemfile`heroku側は下記を追加

{% highlight ruby %}
group :production do  
     gem 'pg'  
end
{% endhighlight %}
    
* `rake assets:precompile RAILS_ENV=production`
* `git add .`
* `git commit -m 'heroku'`
* `heroku create`
* `git push heroku master`
* `heroku rake db:migrate`
* `heroku open`で確認できるはず

##問題
* `heroku rake db:migrate`のエラー
    * `(pg is not part of the bundle. Add it to Gemfile.)`
    * 対策：Gemfileに下記を追加

{% highlight ruby %}
group :production do  
  gem 'pg'  
end
{% endhighlight %}

* `git push heroku master`後、画面で確認すると、エラー画面になってる
    * `heroku logs`で確認すると`ActionView::Template::Error (application.css isn't precompiled)`エラー
    * 対策：`push`前に、`rake assets:precompile RAILS_ENV=production`を実施する
