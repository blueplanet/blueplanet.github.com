---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -10"
date: 2013-01-06 21:07
comments: true
categories: Rails, RSpec, BDD
---
[目录](/blog/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -9](/blog/2013/01/06/ruby-china-clone-9)

## 用户故事

用户希望看到最新回复的信息

帖子列表页面上显示最新回复的信息

- 回复人
- 回复日期

补充：
- 实际的目的是把回复和用户关联起来

## 环境准备

```bash
git checkout -b f10
rails c # console
rails s # server
```

## 步骤

### 编辑`spec/features/topics_spec.rb`，增加最新回复的验证部分

```rb
    scenario '应该显示帖子的最新回复的信息' do
      page.should have_content "last replied by #{@user.name} less than a minute ago"
    end
```

- 测试失败：`expected there to be text "last replied by test_user less than a minute ago" i...`
- 原因：还没有实现具体的代码

### 编辑`topics/_topics.html.haml`

拷贝`ui/topics.html`中最新回复的部分至当前模板

```rb
          %span= "  •  "
          last replied by
          = link_to "knwang", nil, class: "user_link"
          4 mintes ago
```

修改为

```rb
          - last_replay = topic.replies.last
          - if last_replay
            %span= "  •  "
            last replied by
            = link_to last_replay.user.name, last_replay.user, class: "user_link"
            = "#{time_ago_in_words(last_replay.created_at)} ago"
```

在测试用例的`background`部分加入`replay`的数据

```rb
    Topic.last.replies.create content: 'test replay', user: @user
```

- 测试失败：`Can't mass-assign protected attributes: user`
- 原因：没有设置`user`的访问属性

### 编辑`models/reply.rb`，增加用户关联并设置访问属性

```rb
class Reply < ActiveRecord::Base
  attr_accessible :content, :topic, :user

  belongs_to :topic
  belongs_to :user
end
```

### 在`rails console`中执行

```rb
[32] pry(main)> generate "migration AddUserIdToReplies user_id:integer"
      invoke  active_record
      create    db/migrate/20130103120615_add_user_id_to_replies.rb
=> "Completed"
[33] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f10 --no-ff
git branch -d f10
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -11](/blog/2013/01/06/ruby-china-clone-11)