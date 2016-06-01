---
layout: post
title: "processとthread"
date: "2016-06-01 06:24:33 +0900"
---

## 問題
- sidekiqを使っているときに、プロセスやスレッドなどについて迷ってきたので、調べたメモ
  - singletonはどの単位で制御される、プロセス単位？スレッド単位？
  - Mutex.newはどの単位で制御される、プロセス単位？スレッド単位？

## Mutex
- `Mutex`は、確認ソースを書いたら気付きました。当たり前だけど、制御範囲は`Mutex.new`という変数がいるスコープである。
  - 確認コードは別々で実行すると異なるプロセスになり、`m`のスコープはそのプロセスになるので、`m.synchronize`が効く範囲はプロセス範囲になる。

```
# test.rb
process_no = ARGV[0]

m = Mutex.new

5.times do |thread_no|
  Thread.new do
    puts "#{process_no}-#{thread_no} start"

    m.synchronize do
      puts 'loking ...'
      sleep(5)
    end

    puts "#{process_no}-#{thread_no} end"
  end.join
end
```

`ruby test.rb 1`と`ruby test.rb 2`を同時に実行する

## singleton

```
require 'singleton'

class SomeClass
  include Singleton
end

puts "p#{ARGV[0]} start"
a = SomeClass.instance
b = SomeClass.instance
p [a, b]

sleep(5)
puts "p#{ARGV[0]} end"
```

`ruby test.rb 1`と`ruby test.rb 2`と同時実行する

```
ruby test.rb 1
p1 start
[#<SomeClass:0x007fa33d81e218>, #<SomeClass:0x007fa33d81e218>]
p1 end

ruby test.rb 2
p2 start
[#<SomeClass:0x007ffbc2835d40>, #<SomeClass:0x007ffbc2835d40>]
p2 end
```

## 結論
- `singleton`もプロセス範囲内しか効かないので、複数プロセスが動いている環境では要注意

## TODO
- 複数のリクエストが来る場合、裏側は１つのプロセス？
- `unicorn`は１つのプロセス？

## 参考リンク
- [module Singleton (Ruby 2.2.0)](http://docs.ruby-lang.org/ja/2.2.0/class/Singleton.html)
- [class Thread::Mutex (Ruby 2.3.0)](http://docs.ruby-lang.org/ja/2.3.0/class/Thread=3a=3aMutex.html)
