---
layout: post
title: "簡単なプラグインの作る過程"
date: 2012-12-20 22:48
comments: true
tags: 
---

この記事は[Sublime Text 2 Advent Calendar 2012](http://www.adventar.org/calendars/20) の21日目の記事です。

## きっかけ
- `Theme Phoenix`を導入してから、SideBarがもっとカッコよくなったので、もともとずっと非表示してたが、最近表示している時間もどんどん増えています。
- そうなると、SideBarに常に編集中のファイルをフォーカス項目にして頂きたい
- `フォーカス項目`という言葉がわかりづらいかもしれませんが、ようは、下記のように、青色になっている項目のことです

![](https://lh6.googleusercontent.com/-NhQOSIsH4fQ/UNMYmJCyyUI/AAAAAAAACDg/IppMrD7BN6w/s800/Screen%2520Shot%25202012-12-20%2520at%252022.53.33.png)

## 調査過程
- 編集中ファイルで、右クリックのコンテキストメニューには「Revel in Side Bar」というアイテムを発見した！これを実行すると、今のファイルがSideBarのフォーカスアイテムになります。
- なるほど！このコマンドをファイルが切り替えられる時点で呼び出せば行けぞう！
- `SideBarEnhancements`の機能かな、と思って、ソースを検索してみたら、ない！
- じゃ、標準機能みたいね。「Reveal in Side Bar」文言で`Packages\Default`を検索してみたら、あった！

```
    9      { "command": "open_dir", "args": {"dir": "$file_path", "file": "$file_name"}, "caption": "Open Containing Folder…" },
   10      { "command": "copy_path", "caption": "Copy File Path" },
   11:     { "command": "reveal_in_side_bar", "caption": "Reveal in Side Bar" },
   12      { "caption": "-", "id": "end" }
   13  ]
```

- なるほど！コマンドは`reveal_in_side_bar`ですね。実装しよう！

## 実装過程
- ファイルが切り替えられる度に実行したいので、`sublime_plugin.EventListener`を継承
- 実際のコードはは一行だけ！出来ました！おめでとうございます！

```python
import sublime
import sublime_plugin
 
class AutoRevealInSideBar(sublime_plugin.EventListener):
    def on_activated(self, view):
        view.window().run_command("reveal_in_side_bar")
```

- これで、ファイルを切り替える時、どんなに深い階層にあるファイルであっても、ちゃんとSideBarにフォーカスアイテムになるはずです。

## まとめ
- `Sublime Text 2`自体の機能もいろいろコマンドになったりしますので、既存の機能を呼び出したい場合は、まず`Packages\Default`の中を調べましょう
- プラグインを作るのはすっごく簡単です。どんどんやってみましょう！
