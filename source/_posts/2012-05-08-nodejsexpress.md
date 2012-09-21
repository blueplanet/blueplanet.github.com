---
layout: post
title: "Node.js+expressを試してみた"
description: ""
category: Node.js
tags: [Web, JS]
---
[WEB+DB PRESS Vol.68](http://gihyo.jp/magazine/wdpress/archive/2012/vol68)を読んで、Node.jsの実践をやってみた

## インストール ##
- Node.js
  至る簡単で、[Node.js公式サイト](http://nodejs.org/)でインストーラをダウンロード・インストールだけ
 
  
  なるほどねあああああああああ


{% highlight bash %}
$ node -v
v0.6.17
$ npm -v
1.1.21
{% endhighlight %}

- express
    `sudo npm install -g express` (`-g`というオプションはよく分かってない。gemでのアプリケーションではなく、システムにgemをインストールっていう感じかな）
    
## アプリケーション作成 ##
- `express nodeexpress`

{% highlight bash %}
create : nodeexpress
create : nodeexpress/package.json
create : nodeexpress/app.js
create : nodeexpress/public
create : nodeexpress/public/javascripts
create : nodeexpress/public/images
create : nodeexpress/public/stylesheets
create : nodeexpress/public/stylesheets/style.css
create : nodeexpress/routes
create : nodeexpress/routes/index.js
create : nodeexpress/views
create : nodeexpress/views/layout.jade
create : nodeexpress/views/index.jade

dont forget to install dependencies:
$ cd nodeexpress && npm install
{% endhighlight %}

- `cd nodeexpress`
- `sudo npm install`
- `node app.js`でサーバプロセスを起動して、ブラウザで確認出来た！
  すっごくシンプルの画面ですが、何となく感動しました

## コーディングルール ##
- `app.js`でルーティング条件を書く
- `indes.js`でリクエストへ応答内容を書く
- `views/*.jade`でビューのテンプレートを書く

## 感想 ##
- ２００７年頃、最初Railsを触って、このわずかのステップででWEBアプリを作れたの？という感じと似ている
- `express`は`sinatra`に、`Jade`の文法は`haml`に似てる

TODO : Node.js + MongoDBで試す
