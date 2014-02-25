---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -4"
date: 2013-01-06 19:55
comments: true
tags: Rails, RSpec, BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -3](/2013/01/06/ruby-china-clone-3)

## 用户故事

访问者希望看到帖子的详细信息

- 显示标题、内容、节点、创建日期
- 顶部的导航链接

## 环境准备

```bash
git checkout -b f4
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_see_topic_info_spec.rb`

```rb
# coding: utf-8
feature '访问者希望看到帖子的详细信息' do
  background do
    @node = Node.create name: "Ruby"
    @topic = Topic.create title: "topic 1 test", node: @node
  end

  scenario '访问/topics/1, 应该显示帖子的详细信息' do
    visit "/topics/#{@topic.id}"

    page.should have_content @topic.title
    page.should have_content @topic.node.name
  end
end
```

- 测试失败：`No route matches [GET] "/topics/1"`
- 原因：没有`topics/1`对应的路由信息

### 编辑`config/routes.rb`，增加`topics show`的路由设置

```rb
  resources :topics, only: [:index, :show]
```

- 测试失败：`The action 'show' could not be found for TopicsController`
- 原因：`TopicsController`没有定义`show`方法

### 编辑`controllers/topics_controller.rb`，增加`show`方法定义

```rb
  def show
  end
```

- 测试失败：`Missing template topics/show, application/show with {:lo...`
- 原因：没有`show`模板

### 增加`topics/show.html.haml`，拷贝`ui/topic.html.haml`的内容至当前模板

- 测试失败：`expected there to be text "topic 1 test" in "社区 会...`
- 原因：模板中是写死的假数据

### 修改`topics/show.html.haml`内容

```rb
= render 'shared/breadcrumb', links: [["Home", ""], ["社区", ""], ["新手问题", ""], ["浏览帖子", ""]]

%section#show_topic
  %section#topic_banner.box.info-box
    %h1= @topic.title
    %p.topic_info
      = link_to @topic.node.name, @topic.node, class: "node"
      %span= "  •  "
      = link_to "knwang", nil, class: "user_link"
      %span= "  •  "
      = "published #{@topic.created_at} ago"
  %section#topic_content.box
    %p= @topic.content

%section#sidebar
  %section#new_topic.box
    = link_to "发布新帖", nil, class: "btn btn-success"
  %section#stats.box
    %ul
      %li 社区会员: 4029 人
      %li 帖子数: 312 篇
      %li 回帖数: 3123 条
```

**注意：回复的部分在后面的步骤中做，暂时删掉即可**

- 测试失败：`undefined methodtitle' for nil:NilClass``
- 原因：没有对`@topic`变量赋值

### 编辑`controllers/topics_controller.rb`

```rb
  def show
    @topic = Topic.find(params[:id])
  end
```

- 测试失败：`undefined methodcontent' for #<Topic:0x0...``
- 原因：最开始生成`topic`的时候忘了`content`属性了 :)

### 在`rails console`中执行

```rb
[5] pry(main)> generate "migration AddContentToTopics content:text"
      invoke  active_record
      create    db/migrate/20130101134732_add_content_to_topics.rb
=> "Completed"
[6] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

- 测试通过！

### 修改测试用例，增加内容和创建时间的验证

```rb
# coding: utf-8
feature '访问者希望看到帖子的详细信息' do
  background do
    @node = Node.create name: "Ruby"
    @topic = Topic.create title: "topic 1 test", content: "topic 1 content",  node: @node
  end

  scenario '访问/topics/1, 应该显示帖子的详细信息' do
    visit "/topics/#{@topic.id}"

    page.should have_content @topic.title
    page.should have_content @topic.content
    page.should have_content @topic.node.name
    page.should have_content "published #{@topic.created_at} ago"
  end
end
```

- 测试失败：`Can't mass-assign protected attributes: content`
- 原因：没有设置`content`的访问属性

### 编辑`model/topic.rb`

```rb
  attr_accessible :title, :node, :content
```

- 测试通过！

## 再次确认用户故事

### 增加导航栏的验证

```rb
    page.should have_link "Home", href: root_path
    page.should have_link @topic.node.name, href: node_path(@topic.node)
    page.should have_link "浏览帖子", href: topic_path(@topic)
```

- 测试失败：`undefined local variable or methodroot_path' for...``n
- 原因：没有`root_path`的定义

### 编辑`config/routes.rb`，增加`root_path`的定义

```rb
MyRubyChina::Application.routes.draw do
  resources :topics, only: [:index, :show]
  resources :nodes, only: [:show]

  root to: "topics#index"

  match 'ui/:action', controller: 'ui'
...
```

#### 删除默认的主页文件

```bash
rm public/index.html
```

- 测试失败：`expected to find link "Home" but there were no matches. Als...`
- 原因：没有定义对应的链接

### 编辑`topics/show.html.haml`，修改顶部导航栏链接

```rb
= render 'shared/breadcrumb', links: [["Home", root_path], [@topic.node.name, @topic.node], ["浏览帖子", @topic]]
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git rm public/index.html
git commit 
git checkout dev
git merge f4 --no-ff
git branch -d f4
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -5](/2013/01/06/ruby-china-clone-5)
