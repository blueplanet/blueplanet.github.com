# 新しいサーバ構築手順

## 全体手順

1. ansibleでベース構築
2. deployユーザに作業用のssh公開鍵を設定しておく
3. cap 実行してアプリの環境を構築
   1. ​

## 詳細



1. ansible でベース構築

   1. base package install

   2. rbenv

   3. nginx

      - AWS Linux でインストールした nginx の設定ファイルは、 **unicornにリクエストしない** 
      - 邪魔だったのは、`default_server` のようでした。
      - 解決：ansibleで default_serverを削除すれば、普通に unicorn にリクエストされました

      ```ruby
      ...
        
      http {
      	...

          include /etc/nginx/conf.d/*.conf;

          index   index.html index.htm;

          server {
              listen       80 default_server;
              listen       [::]:80 default_server;
              server_name  localhost;
              root         /usr/share/nginx/html;

              # Load configuration files for the default server block.
              include /etc/nginx/default.d/*.conf;

      		...
            }
        }
      ```

   4. app

2. capistrano 導入する

   1. gem 追加
   2. `bundle exec cap …` で設定ファイル生成

3. cap 実行

   1. 初回実行だと、database.yml ないので、そこで止まる
   2. `bundle exec cap deploy:upload` でファイルアップロードし、値を設定する
   3. `database.yml / .env` 
   4. 別所で良いので `bundle exec rake secret` で SECRET_KEY_BASEの値を生成する