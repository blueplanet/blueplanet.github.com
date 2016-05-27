---
layout: post
title: "dockerでsshアクセスできるコンテナを作ってみる"
date: "2016-05-27 14:34:35 +0900"
---

ローカルでAnsibleの内容を確認したりしたい。現状はVagrantを使ってますが、Dockerでもっと軽く動けるかどうかを試してみる

## 環境
- MacOS 10.112
- Docker beta for Mac(Version 1.11.1-beta13 (build: 7975) 16dbe555c7dd4304521b21e8286d83fe4864c15c)

## Dockerfile
[DockerコンテナにSSHする際に環境変数を引き継ぐ - Qiita](http://qiita.com/stbmp23/items/f7cb9460bf3c8150d112) の前半を参考して、一番シンプルな内容にする

```
FROM centos:7

RUN yum install -y openssh-server initscripts
RUN /usr/sbin/sshd-keygen

RUN echo 'PermitEmptyPasswords yes' >> /etc/ssh/sshd_config
RUN useradd deploy
RUN passwd -u -f deploy

CMD /usr/sbin/sshd -D
```

## 起動

```
docker build -t sshtest .
docker run -p 1022:22 -d sshtest
```


## sshで接続してみる

```
ssh deploy@localhost -p 1022
```

## Docker beta for Mac
メニューからSettings画面を開くと、GUIでdocker全体が使うメモリとCPUを設定できる、便利！


![Docker Mac]({{site.baseurl}}/images/docker-mac.png)
