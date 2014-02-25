---
layout: post
title: "OS 10.8 gem install json faild"
date: 2012-10-04 06:25
comments: true
tags: 
---

## 環境

- Mac OS 10.8.2
- ruby 1.9.2p320
- gem 1.8.24
- xcode 4.5 (4G182)

## 問題

`gem install json`を実行する場合、下記のエラーで落ちる

```bash
$ gem install json                                                                       *[master][1.9.2-p320] 
Fetching: json-1.7.5.gem (100%)
Building native extensions.  This could take a while...
ERROR:  Error installing json:
  ERROR: Failed to build gem native extension.

        /Users/gyo/.rbenv/versions/1.9.2-p320/bin/ruby extconf.rb
creating Makefile

make
/usr/bin/gcc-4.2 -I. -I/Users/gyo/.rbenv/versions/1.9.2-p320/include/ruby-1.9.1/x86_64-darwin11.3.0 -I/Users/gyo/.rbenv/versions/1.9.2-p320/include/ruby-1.9.1/ruby/backward -I/Users/gyo/.rbenv/versions/1.9.2-p320/include/ruby-1.9.1 -I. -DJSON_GENERATOR -I'/Users/gyo/.rbenv/versions/1.9.2-p320/include'  -D_XOPEN_SOURCE -D_DARWIN_C_SOURCE   -fno-common -O3 -ggdb -Wextra -Wno-unused-parameter -Wno-parentheses -Wpointer-arith -Wwrite-strings -Wno-missing-field-initializers -Wshorten-64-to-32 -Wno-long-long  -pipe -O3 -Wall -O0 -ggdb  -o generator.o -c generator.c
i686-apple-darwin11-gcc-4.2.1: error trying to exec 'cc1': execvp: No such file or directory
make: *** [generator.o] Error 1


Gem files will remain installed in /Users/gyo/.rbenv/versions/1.9.2-p320/gemsets/ruby-china/gems/json-1.7.5 for inspection.
Results logged to /Users/gyo/.rbenv/versions/1.9.2-p320/gemsets/ruby-china/gems/json-1.7.5/ext/json/ext/generator/gem_make.out
```

## 解決

1. xcode で`Command Line Tools`をインストール
2. `sudo ln -sf /usr/bin/llvm-gcc-4.2 /usr/bin/gcc-4.2`を実行

---
参考URL

- [OS X 10.8 - error trying to exec '/usr/bin/i686-apple-darwin11-gcc-4.2.1' - installing json gem](http://stackoverflow.com/questions/11710568/os-x-10-8-error-trying-to-exec-usr-bin-i686-apple-darwin11-gcc-4-2-1-inst)
