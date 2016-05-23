---
layout: post
title: "Electronでpomotodoのアプリを作る"
date: "2016-05-23 16:39:27 +0900"
---

最近ポモドーロテクニックを導入しています。
https://pomotodo.com/ を利用していますが、悩んでるのは、公式のアプリがちょっと不便だったりします。

## 公式アプリの不便なところ
- ある程度ショートカットキーがサポートされていますが、キーを押下すると音がする、そしてそれをオフする設定がない
  - ポモドーロが終わった時点とか、休憩時間が終わった時点とか音を出してもらいたいので、システム全体の音をオフするのはできないので、ちょっとうるさいですね。
- WEB版しかない機能があったりします。
  - たとえば、`#dev`とかでタグをつけると、色が変わったりしますが、アプリのほうは機能しない

## Electronでpomotodoのアプリを作ってみる
- [Electron で AWS Management Console をデスクトップアプリ化してみた ｜ Developers.IO](http://dev.classmethod.jp/cloud/electron-aws-mc/) ほぼこの記事のままで、URLを `https://pomotodo.com`に修正しただけ
- MacOSのアプリパッケージを作成するまわりは、 [30分で出来る、JavaScript (Electron) でデスクトップアプリを作って配布するまで - Qiita](http://qiita.com/nyanchu/items/15d514d9b9f87e5c0a29#%E3%82月曜日2%E3%83%97%E3%83月曜日A%E3%825月1%E3%835月C%E3%825月7%E3%83月曜日7%E3%835月3%E3%82%92%E3%82月曜日2%E3%835月C%E3%82月曜日B%E3%82月曜日4%E3%83%96%E5%8C%96%E3%81%99%E3%82%8B) を参考しました。
- できたもの https://github.com/blueplanet/pomotodo-e

![Pomotodo-e]({{site.baseurl}}/images/pomotodo-e.png)
