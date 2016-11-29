---
layout: post
title: "Markdownでシーケンス図を書きたい"
date: 2016-11-30
---

調べてみたら、MWeb自体はサポートされていた。

- まず設定で使用するように設定しておく

    ![mweb_chart](/images/mweb_chart.png)

- その後は言語を `sequence` か、`flow`かを指定すれば書ける

>
    ```sequence
    Andrew->China: Says Hello
    Note right of China: China thinks about it
    China-->Andrew: How are you?
    Andrew->>China: I am good thanks!
    ```

>
    ```flow
    st=>start: Start:>http://www.google.com[blank]
    e=>end:>http://www.google.com
    op1=>operation: My Operation
    sub1=>subroutine: My Subroutine
    cond=>condition: Yes
    or No?:>http://www.google.com
    io=>inputoutput: catch something...
>    
    st->op1->cond
    cond(yes)->io->e
    cond(no)->sub1(right)->op1
    ```


![mweb_chart_result](/images/mweb_chart_result.png)

- 描画するのは、`http://bramp.github.io/js-sequence-diagrams/, http://adrai.github.io/flowchart.js/`なので、文法はそちらを参照すればよい
- 面倒なのは、画像を自分で保存しないといけない
- 個人的には [mermaid.js](http://knsv.github.io/mermaid/)のほうがかっこいいなと思ったりする

