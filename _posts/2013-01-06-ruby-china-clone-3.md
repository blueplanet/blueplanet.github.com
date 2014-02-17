---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -3"
date: 2013-01-06 17:44
comments: true
categories: Rails, RSpec, BDD
---

[目录](/blog/2013/01/06/ruby-china-clone-cover/)
上一步：[使用 RSpec+Capybara 简单BDD入门 -2](/blog/2013/01/06/ruby-china-clone-2/)

## 用户故事

访问者希望看到一个节点的帖子列表

- 点击节点后显示该节点的帖子列表

## 环境准备

```bash
git checkout -b f3
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/nodes_spec.rb`

```rb
# coding: utf-8
feature '访问者希望看到一个节点的帖子列表' do
  background do
    @node1 = Node.create name: "Ruby Node"
    @node2 = Node.create name: "Rails Node"

    2.times.map.with_index { |i| Topic.create title: "topic #{i}", node: @node1}
    3.times.map.with_index { |i| Topic.create title: "topic #{i + 10}", node: @node2}
  end

  scenario '点击节点名称后，应该显示该节点的帖子列表' do
    visit '/topics'

    page.should have_content @node1.name
    page.should have_content @node2.name

    first(:link, @node1.name).click

    page.should have_content @node1.name
    page.should_not have_content @node2.name
  end
end
```

- 测试失败：`Failure/Error: page.should_not have_content @node2.name expected there not to be text "Rails Node" in "....`
- 原因：节点的链接还是无效链接，没有起作用

### 修改`topics/index.html.haml`

```rb
= link_to topic.node.name, nil, class: "node"
```

修改为

```rb
= link_to topic.node.name, , class: "node"
```

- 测试失败：`undefined method node_path' for ...`
- 原因：没有定义`node`的路由信息

### 修改`config/routes.rb`，增加`node`的路由定义

```rb
  resources :nodes, only: [:show]
```

- 测试失败：`uninitialized constant NodesController`
- 原因：没有node controller

### 在`rails console`中执行

```rb
[3] pry(main)> generate "controller nodes"
      create  app/controllers/nodes_controller.rb
      invoke  haml
      create    app/views/nodes
=> "Completed"
[4] pry(main)>
```

- 测试失败：`The action 'show' could not be found for NodesController`
- 原因：没有`show`方法

### 编辑`controllers/nodes_controller.rb`，增加`show`方法

```rb
class NodesController < ApplicationController
  def show
  end
end
```

- 测试失败：`Missing template nodes/show, application/show with {:...`
- 原因：没有`show`模板

### 新建`views/nodes/show.html.haml`

拷贝`ui/node.html`至上述文件，并修改第5行

```rb
    %h2= node.name
```

- 测试失败：`undefined methodname' for nil:NilClass`
- 原因：没有对`@node`赋值

### 编辑`controllers/nodes_controller.rb`，增加`@node`的赋值

```rb
    @node = Node.find(params[:id])
```

- 测试成功！但只是因为都是写死的数据，所以需要继续修改

### 修改`nodes/show.html.haml`第7行

```rb
%p= @node.description
```

- 测试失败：`undefined methoddescription' for #<...``
- 原因：`node`没有`description`方法

### 在`rails console`中执行

```rb
[4] pry(main)> generate "migration AddDescriptionToNodes description:text"
      invoke  active_record
      create    db/migrate/20130101042100_add_description_to_nodes.rb
=> "Completed"
[5] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

- 测试成功。但各个帖子的信息还是写死的数据

### 编辑`controllers/nodes_controller.rb`，修改帖子列表的部分

```rb
= render 'shared/breadcrumb', links: [["Home", ""], ["社区", ""], ["Ruby", ""]]

%section#node_topics
  %section#node_info.box.box-gray.info-box
    %h2= @node.name
    %span 共有48个讨论主题
    %p= @node.description
  %ul.topics.box
    - @node.topics.each do |topic|
      %li
        %a.span1(href="")
          %img(src="#{gravatar_url('joe@example.com')}")
        %article.span7
          %p.topic_title
            = link_to topic.title
          %p.topic_info
            = link_to topic.node.name, topic.node, class: "node"
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

- 测试失败：`undefined methodtopics' for #<Node:...``
- 原因：`node`没有`topics`方法

### 编辑`model/node.rb`，增加`topics`的声明

```rb
  has_many :topics
```

测试成功！

## 重构

仔细看一下，`topics/index.html.haml`和`nodes/show.html.haml`两个页面中，显示帖子列表的部分是完全相同的，可以提出一个部分模板

### 新建`topics/_topics.html.haml`

```
%ul.topics.box
  - topics.each do |topic|
    %li
      %a.span1(href="")
        %img(src="#{gravatar_url('joe@example.com')}")
      %article.span7
        %p.topic_title
          = link_to topic.title
        %p.topic_info
          = link_to topic.node.name, topic.node, class: "node"
          %span= "  â€˘  "
          = link_to "knwang", nil, class: "user_link"
          %span= "  â€˘  "
          = "published #{time_ago_in_words(topic.created_at)} ago"
      %p.replies.span1
        %span 12
```

** 注意：把`@topics`变量修改为本地变量`topics`**

### 修改`topics/index.html.haml`

把文件中上述部分删除后，增加部分模板的调用

```rb
    %span 查看： 默认
  = render 'topics', topics: @topics

%section#sidebar
  %section#new_topic.box
```

### 修改nodex/show.html.haml

把文件中上述部分删除后，增加部分模板的调用

```rb
    %span 共有48个讨论主题
    %p= @node.description
  = render 'topics/topics', topics: @node.topics

%section#sidebar
  %section#new_topic.box
```

**注意：传入变量的部分略有不同**

- 测试应该正常全部通过

## 完善spec

### 编辑`spec/features/nodes_spec.rb`，增加`description`的设置以及验证

```rb
# coding: utf-8
feature '访问者希望看到一个节点的帖子列表' do
  background do
    @node1 = Node.create name: "Ruby Node", description: "Ruby是一门优美的语言"
    @node2 = Node.create name: "Rails Node", description: "Rails是一个快速WEB开发框架"

    2.times.map.with_index { |i| Topic.create title: "topic #{i}", node: @node1}
    3.times.map.with_index { |i| Topic.create title: "topic #{i + 10}", node: @node2}
  end

  scenario '点击节点名称后，应该显示该节点的帖子列表' do
    visit '/topics'

    Node.all.each do |node|
      page.should have_content node.name
    end

    first(:link, @node1.name).click

    page.should have_content @node1.name
    page.should have_content @node1.description

    page.should_not have_content @node2.name
    page.should_not have_content @node2.description
  end
end
```

- 测试失败：`Can't mass-assign protected attributes: description`
- 原因：`description`的访问属性没有设置

好险，这个地方忘记了！

### 编辑`model/node.rb`，增加`description`属性声明

```rb
  attr_accessible :name, :description
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f3 --no-ff
git branch -d f3
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -4](/blog/2013/01/06/ruby-china-clone-4)