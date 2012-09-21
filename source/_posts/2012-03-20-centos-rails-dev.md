---
layout: post
title: "Amazon EC2 のCentOSでRails開発環境構築"
date: 2012-03-20 15:43
comments: true
categories: 
- AWS
- クラウド
- Rails
- 日本語
---

　Rails勉強中。やはり手で動かないと、理解できたとしても、それほどの知識を覚えるのは無理なので、いざ何かを試したい時、手元に環境がないと困ることがよくあります。

　ちょうど、AWSは、新規登録だと、[無料で１年間利用できるプログラム](http://aws.amazon.com/jp/free/)があります。そこに開発環境をインストールしておけば、SSH接続さえ出来れば、開発環境が使えるので、うれしいかなと思うので、試しに作ります。

***EC2の作成は普通にAmazonのLinuxでマイクロインスタンスを作成***

## OS周りの設定
* `sudo yum install -y zsh git`
* rootパスワードを設定　`sudo passwd`
* ec2-userのパスワードを設定（初期パスワードがわからないので、Rootユーザに切り替え、設定する）  
`su root`  
`passwd ec2-user`
* shをzshに切り替え  
  `chsh -s /bin/zsh`
  
## oh-my-zsh(**個人のバージョン**)導入  
* `git clone git://github.com/blueplanet/oh-my-zsh.git ~/.oh-my-zsh`
* `cd ~/.oh-my-zsh`  
* `git submodule init`
* `git submodule update`

## RVM導入
* `zsh -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)`
* > zsh: command not found: shopt  
set: no such option: errtrace  
      
  が出てしまったのですが、影響がなさそうなので、無視
* `source ~/.zshrc`を実行し、設定を反映し、`rvm`で確認できた！
    
## Ruby && Rails
* `sudo yum install -y rubygems gcc-c++ make patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel`
* `rvm install 1.9.3` ***←　マイクロインスタンスで実行すると、結構時間がかかる***
* `rvm use 1.9.3 --default`
* `rvm gemset create r3`
* `rvm use 1.9.3@r3 --default`
* `gem install rails --no-rdoc --no-ri`

`rails -v`を実行したら、`Rails 3.2.2`ができている！！

***WindowsからのSSH接続は、後で。***