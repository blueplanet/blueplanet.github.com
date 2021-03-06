---
layout: post
title: "実践 Rails コードチャレンジに参加しました"
date: 2012-12-20 21:27
comments: true
tags: 
---
昨日、[【Forkwell＆CodeIQ共催】実践 Rails コードチャレンジ (松田明さん監修)](http://atnd.org/event/ForkwellCodeIQ) に参加しました

## きっかけ
- イベントを分かってから、参加するかどうかを悩んでました。Rails素人なので、多分回答出来ないかと思いました
- でも、やはり @a_matsuda さんのお話しを聞きたいですね。特に Rails 4 
- ので、応募しちゃいました

## 過程
イベンとの前問題が公開されたので、17日夜からチャレンジしてみました

### 第一問
- 最初は1問だけだと思いました。。。。。
- SQLチューニングのお話しで、例のN+1の問題なので、これなら回答できそうと。
- `counter cache`機能は分かってたが、細かい使い方は調べて回答しました。`migration`ファイルの書き方も後ほど気づいて、既存のカウント数を設定したり、`reset_column`したり追加しました
- 回答は [ gist reversions ](https://gist.github.com/4322792/revisions) で
- ギリギリ前日までに回答をアップロードしたら、第二問が！！！
- マジ！しかも、細かい指示がなく、「コードの負債をソースレビュー視点から直してください」という
- まあ、素人ですし、1問できてるし、残りは他の方の回答を勉強すればという気持ちになった

### 第二問
- いろいろ見ても分からなかったので、`PublicationController`だけを読んで、ある重複コードを抽出だけでした
- コードはここ [github commit ](https://github.com/blueplanet/forkwell/commit/252a67b133e6e1fc1205968ba3653be14627734c)

### 第三問
- 何となくのぞってみたいので、第二問のとんでもないものをアップしてみました
- `Rails 4 のstrong parametersで。。。。`とかなんとか、`strong parameters`ってなんでしょうか？
- 全く分からないので、諦めて、同じテーブルの同士とちょっとワイワイ話してみました。楽しかった

### 発表
- 最後は、参加者が自分の回答を発表して、 @a_matsuda 先生にレビューしてもらえました
- 1問目の回答は、やはりドキドキしちゃったので、応募出来なかった
    - 回答した方は`counter cache`を使った方もいて、 @a_matsuda 先生に「これは満点」と褒められて、そして！なんと、github のTシャツがもらった！応募しなかった僕はすごく悔しかった！
- 2問目は、1問目のプレゼントを見てすごく欲しいので、何とかしたいが、自分のものがとんでもない修正なので、あんまり勇気がなかった
- が、二人の方がいろいろ発表したのですが、二人共僕の箇所に気づいてなかった、そして、うちのテーブルには、発表は１つもなかったので、やはり勇気を出して発表して見ました。
- なんと、@a_matsuda　先生に「おお、このやり方は DHH らしいですね！」と褒められた！すごく嬉しかった！
- あと、一番重要なのは、github のカップを貰いました！ありがとうございました！  

![](https://lh4.googleusercontent.com/-iaohwghL2DU/UNMT07j553I/AAAAAAAACDE/PNcOyQVs2hk/s800/github_cup.png)

## @a_matsuda 先生から Rails 4 関連のお話し
- `strong parameters`の説明
    - なるほど。`mass assignment`の対策ですね。`model`の責任じゃないよ、`controller`ちゃんとやれ！的な感じですかね
- [github: asakusarb / action_args](https://github.com/asakusarb/action_args)
    - @a_matsuda 先生の gem 。細かい機能はまだ理解出来てないので、後で調べる

## まとめ
- 人数ちょっと少なかった気がしますが、気分はすごく良かったです。またチャンスがありましたら、また参加したいですね
- @a_matsuda 先生は意外に優しかった。←　いつか @a_matsuda 先生が厳しいと誤解したんだろうね
- もっと @a_matsuda 先生の技を勉強したいです！
- [github: CodeIQ/forkwell](https://github.com/CodeIQ/forkwell) 今回チャレンジ用のリポジトリに、特に`question_2`から出てきた`RSpec`の`requests`スペックの書き方は大変勉強になりました。
    - 最近はちょうど`request`の書き方にハマっているので、すごく嬉しいです！
    - 最初は`requests`で`rspec`実行すると、すべて赤だったが、`jnicklas/capybara`の `Ver2.0`以降は、`features`じゃないとダメという仕様変更でした。

