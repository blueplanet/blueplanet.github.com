---
layout: post
title: "RubyでTwitter APIを使う"
description: ""
category: Twitter
tags: [Ruby]
---
RailsでTwitterを使いたいので、まずは、RubyでTwitterを使う手段を調べてみた

- `gem install twitter` Rubyの場合は、これがよく使われるらしい

パブリックタイムラインの場合、認証なくてもOK

    require 'twitter'
    
    Twitter.user_timeline("aoi_wakusei", :count => 200).map do |tweet|
      "#{tweet.created_at} : #{tweet.text}"
    end
      
確かにこんなに簡単に使えるのは感動したが、期間を指定したいが、使い方はまだ見つけていない

TODO : Twitter API期間指定してタイムライン検索
