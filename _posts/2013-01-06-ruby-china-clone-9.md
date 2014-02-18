---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -9"
date: 2013-01-06 21:04
comments: true
categories: Rails, RSpec, BDD
---
[目录](/blog/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -8](/blog/2013/01/06/ruby-china-clone-8)

## 用户故事

用户希望看到帖子的回复列表

- 显示帖子的回复数量
- 显示回复的下列信息
    - 回复日期时间
    - 回复内容

## 环境准备

```bash
git checkout -b f9
rails c # console
rails s # server
```

## 步骤

### 编辑`spec/features/guest_can_see_topic_info_spec.rb`，增加回复部分的验证

```rb
  scenario '应该显示帖子的回复信息' do
    visit "/topics/#{@topic.id}"

    page.should have_content "共收到 5 条回复"
  end
```

- 测试失败：`expected there to be text "共收到 5 条回复" in "社区 会员...`
- 原因：还没有实现代码

### 编辑`topics/show.html.haml`

拷贝`ui/topic.html.haml`的回复部分到当前模板

```rb
  %section#topic_content.box
    %p= @topic.content

  %section#replies_banner.box.info-box
    %span 共收到 5 条回复

  %section#replies.box
    %ul
      %li
        %a.span1(href="")
          %img(src="#{gravatar_url('joe@example.com')}")
        %article.span8
          = link_to "knwang", nil, class: "user_link"
          %span two hours ago
          %p ++ -- 这个操作就是罪恶的源泉。。++ -- 这个操作就是罪恶的源泉。。++ -- 这个操作就是罪恶的源泉。。++ -- 这个操作就是罪恶的源泉。。++ -- 这个操作就是罪恶的源泉。。++ -- 这个操作就是罪恶的源泉。。
      %li
        %a.span1(href="")
          %img(src="#{gravatar_url('joe@example.com')}")
        %article.span8
          = link_to "knwang", nil, class: "user_link"
          %span two hours ago
          %p ++ -- 这个操作就是罪恶的源泉。。
      %li
        %a.span1(href="")
          %img(src="#{gravatar_url('joe@example.com')}")
        %article.span8
          = link_to "knwang", nil, class: "user_link"
          %span two hours ago
          %p ++ -- 这个操作就是罪恶的源泉。。


%section#sidebar
  %section#new_topic.box
```

并修改为

```rb
  %section#topic_content.box
    %p= @topic.content

  %section#replies_banner.box.info-box
    %span= "共收到 #{@topic.replies.count} 条回复"

  %section#replies.box
    %ul
      - @topic.replies.each do |reply|
        %li
          %a.span1(href="")
            %img(src="#{gravatar_url('joe@example.com')}")
          %article.span8
            = link_to "knwang", nil, class: "user_link"
            %span= "#{time_ago_in_words(reply.created_at)} ago"
            %p= reply.content 

%section#sidebar
  %section#new_topic.box
```

**注意：回复的`user`暂时先不考虑**

- 测试失败：`undefined methodreplies' for #Topic:0x007fc0f5c4b3b0...``
- 原因：没有`replies`定义

### 编辑`models/topic.rb`，增加关联定义

```rb
  has_many :replies
```

- 测试失败：`uninitialized constant Topic::Reply`
- 原因：没有`Reply`这个`model`的定义

### 在`rails console`中执行

```rb
[26] pry(main)> generate "model reply content:text topic_id:integer"
      invoke  active_record
      create    db/migrate/20130103064335_create_replies.rb
      create    app/models/reply.rb
=> "Completed"
[27] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

### 修改spec文件

```rb
# coding: utf-8
feature '访问者希望看到帖子的详细信息' do
  background do
    @node = Node.create name: "Ruby"
    @topic = Topic.create title: "topic 1 test", content: "topic 1 content",  node: @node

    5.times.map.with_index { |i| Reply.create content: "reply #{i}", topic: @topic, user: user }
  end

  scenario '应该显示帖子的详细信息' do
......
  end

  scenario '应该显示帖子的回复信息' do
    visit "/topics/#{@topic.id}"

    page.should have_content "共收到 #{@topic.replies.count} 条回复"

    @topic.replies.each do |reply|
      page.should have_content reply.content
    end
  end
end
```

- 测试失败：`Can't mass-assign protected attributes: topic, user`
- 原因：没有设置`topic,user`的访问属性

### 编辑`models/reply.rb`，增加访问属性并设置topic和user的关联

```rb
class Reply < ActiveRecord::Base
  attr_accessible :content, :topic

  belongs_to :topic
end
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f9 --no-ff
git branch -d f9
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -10](/blog/2013/01/06/ruby-china-clone-10)