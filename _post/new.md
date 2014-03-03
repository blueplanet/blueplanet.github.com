# StackEdit+Jekyll+GithubPageのブログワークフロー

## StackEditとは

- オープンソースのオンラインMarkdownエディター
- 

## vimキーバインド設定

```js
userCustom.onReady = function() {
    var ace = {}
    ace.require = require
    ace.define = define
    ace.require(["ace/lib/net"], function(acenet) {
        acenet.loadScript("https://rawgithub.com/ajaxorg/ace-builds/master/src-min-noconflict/keybinding-vim.js", function() {
            e = document.querySelector(".ace_editor").env.editor
            ace.require(["ace/keyboard/vim"], function(acevim) {
                e.setKeyboardHandler(acevim.handler);
            });
        });
    });
    window.ace = ace;
};
```



## 参考URL
[stackedit/issues/254][1]


  [1]: https://github.com/benweet/stackedit/issues/254