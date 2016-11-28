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


```sequence
Andrew->China: Says Hello
Note right of China: China thinks about it
China-->Andrew: How are you?
Andrew->>China: I am good thanks!
```

```flow
st=>start: Start:>http://www.google.com[blank]
e=>end:>http://www.google.com
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?:>http://www.google.com
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```



