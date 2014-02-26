---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -1"
date: 2013-01-06 17:17
comments: true
tags: Rails RSpec BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)

## 用户故事

访问者希望看到所有帖子的列表

- 显示帖子的标题和创建时间

## 环境准备

```bash
git checkout -b f1
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_see_all_topics_spec.rb`

```rb
# coding: utf-8
feature '访问者希望看到所有帖子的列表' do
  scenario '访问/topics, 应该显示所有帖子' do
    visit '/topics'

    page.should have_content "DHH 的公开课"
    page.should have_content "Rails3 中 compass 的 IE 使用问题"
    page.should have_content "这周二上海搞Ruby Tuesday么？"
  end
end
```

- 测试失败：No route matches [GET] "/topics"
- 原因：没有正确的路由设置信息

### 编辑`config/route.rb`, 增加`topics`的定义

```rb
  resources :topics, only: [:index]
```

- 测试失败：`uninitialized constant TopicsController`
- 原因：没有定义`TopicsController`

### 在`rails console`中执行

```rb
[1] pry(main)> generate "controller topics"
      create  app/controllers/topics_controller.rb
      invoke  haml
      create    app/views/topics
=> "Completed"
```

- 测试失败：`The action 'index' could not be found for TopicsController`
- 原因：`TopicsController`中没有定义`index`方法

### 编辑 `topics_controller.rb`

```rb
class TopicsController < ApplicationController
  def index
  end
end
```

- 测试失败：`Missing template topics/index, application/index ...`
- 原因：没有`topics/index`模板


### 新建文件：`app/views/topics/index.html.haml`

- 测试失败：`Failure/Error: page.should have_content "DHH 的公开课"`
- 原因：模板中没有显示内容

拷贝`ui/topics.html.haml`的内容至当前模板

- 测试成功了，但是不要高兴！成功是因为页面上所有的数据都是写死的假数据。必须更改为正确的代码

### 编辑`app/views/topics/index.html.haml`

```rb
= render 'shared/breadcrumb', links: [["Home", ""], ["社区", ""]]

%section#topics
  %section#topics_info.box.box-gray.info-box
    %span 查看： 默认
  %ul.topics.box
    - @topics.each do |topic|
      %li
        %a.span1(href="")
          %img(src="#{gravatar_url('joe@example.com')}")
        %article.span7
          %p.topic_title
            = link_to topic.title
          %p.topic_info
            = link_to "分享", nil, class: "node"
            %span= "  •  "
            = link_to "knwang", nil, class: "user_link"
            %span= "  •  "
            = "published #{time_ago_in_words(topic.created_at)} ago"
        %p.replies.span1
          %span 12

%section#sidebar
  %section#new_topic.box
    = link_to "发布新帖", nil, class: "btn btn-success"
  %section#stats.box
    %ul
      %li 社区会员: 4029 人
      %li 帖子数: 312 篇
      %li 回帖数: 3123 条
```

- 测试失败：`undefined methodeach' for nil:NilClass``
- 原因：`TopicsController`的`index`方法中没有对`@topics`赋值

### 编辑`topics_controller.rb`，增加赋值部分

```rb
class TopicsController < ApplicationController
  def index
    @topics = Topic.all
  end
end
```

**注意：由于是示范项目所以直接使用了 Topic.all ， 实际项目中绝对不能这样做**

- 测试失败：`uninitialized constant TopicsController::Topic`
- 原因：没有 `Topic` 这个 `model`

### 在`rails conssole`中执行

```rb
[2] pry(main)> generate "model topic title"
      invoke  active_record
      create    db/migrate/20121229122838_create_topics.rb
      create    app/models/topic.rb
=> "Completed"
[3] pry(main)>
```

- 测试失败：`expected there to be text "DHH 的公开课" in "社区...`
- 原因：测试数据库中没有对应的数据

### 编辑 `spec/features/guest_can_see_all_topics_spec.rb`，增加`background`部分

```rb
# coding: utf-8
feature '访问者希望看到所有帖子的列表' do
  background do
    Topic.delete_all

    Topic.create title: "DHH 的公开课"
    Topic.create title: "Rails3 中 compass 的 IE 使用问题"
    Topic.create title: "这周二上海搞Ruby Tuesday么？"
  end

  scenario '访问/topics, 应该显示所有帖子' do
    visit '/topics'

    Topic.all.each do |topic|
      page.should have_content topic.title
    end
  end
end
```

- 测试成功！！完成！！

## 提交代码

git add .
git commit
git checkout dev
git merge f1 --no-ff
git push origin dev

## 总结

- 执行测试之后，按照提示的错误进行修改即可
- 问题
    - 目前自动测试基本不起作用，每次都需要手动执行测试

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -2](/2013/01/06/ruby-china-clone-2)
