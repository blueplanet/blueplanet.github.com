---
layout: post
title: "Jekyll Markdownでのコードブロック"
description: ""
category: Blog
tags: [Jekyll, Markdown]
---
Jekyllでのソースの書き方の問題はずっと解決出来てない状態。

今日わかったのは、４つのスペースでインデントを付けると、色はないけど、ちゃんとそのまま表現される

{% highlight python linenos %}
def test
  aaa = "aaa"
end
{% endhighlight %}

これは、Markdownで前後とも一行の空白行があるわけ。ないとこの３行が１行になってしまう。前の行末に２つのスペースを入れると、改行はされますが、やはり１行になってしまう。  
    def test
      aaa = "aaa"
    end

とりあえずは、これで行きます。

TODO : Markdownのソースブロックの色
