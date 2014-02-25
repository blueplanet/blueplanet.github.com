---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -2"
date: 2013-01-06 17:41
comments: true
tags: Rails, RSpec, BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -1](/2013/01/06/ruby-china-clone-1)

## 用户故事

访问者希望登录

- 使用用户名登录
- 登录后显示帖子列表
- 在右上角显示用户名

## 环境准备

```bash
git checkout -b f7
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_sign_in_spec.rb`

```rb
# coding: utf-8
feature '访问者希望登录' do
  background do
    @user = User.create name: "test_user", email: "test@test.com"
  end

  scenario '输入用户名后点击"登录"按钮，应该正常登录' do
    visit "/sign_in"

    fill_in "user_name", with: @user.name
    click_button "登录"

    current_path.should == root_path
  end
end
```

- 测试失败：`No route matches [GET] "/sign_in"`
- 原因：没有`sign_in`的路由信息

### 编辑`config/routes.rb`，增加路由信息

```rb
  match 'sign_in', to: "sessions#new"
```

- 测试失败：`uninitialized constant SessionsController`
- 原因：没有`sessions`这个`controller`

### 在`rails console`中执行

```rb
[10] pry(main)> generate "controller sessions"
      create  app/controllers/sessions_controller.rb
      invoke  haml
      create    app/views/sessions
=> "Completed"
[11] pry(main)>
```

- 测试失败：`The action 'new' could not be found for SessionsController`
- 原因：没有定义`new`方法

### 编辑`controllers/sessions_controller.rb`，增加`new`方法

```rb
class SessionsController < ApplicationController
  def new
  end
end
```

- 测试失败：`Missing template sessions/new, application/new with {:lo...`
- 原因：没有对应的`new`模板

### 新建`sessions/new.html.haml`模板

拷贝`ui/signin.html`内容至当前模板

```rb
= render 'shared/breadcrumb', links: [["Home", ""]]

%section#sign_in.box
  %h1 登录
  %form(action="" method="post" class="form-horizontal")
    .control-group
      %label(class="control-label") 用户名
      .controls
        %input(type="text")
    .submit-button
      %input(type="submit" value="登录" class="btn btn-primary")

%section#sidebar
  %section#has_account.box
    %h2 还没有帐号?
    = link_to "注册"
```

- 测试失败：`Unable to find field "user_name"`
- 原因：模板还是写死的假数据

### 修改`sessions/new.html.haml`模板

```rb
= render 'shared/breadcrumb', links: [["Home", ""]]

%section#sign_in.box
  %h1 登录
  = form_tag sessions_path, method: :post, class: "form-horizontal" do
    .control-group
      = label_tag :user_name, "用户名", class: "control-label"
      .controls
        = text_field_tag :user_name
    .submit-button
      = submit_tag "登录", class: "btn btn-primary"

%section#sidebar
  %section#has_account.box
    %h2 还没有帐号?
    = link_to "注册"
```

- 测试失败：`undefined local variable or methodsessions_path' for #<...``
- 原因：没有`sessions#create`的路由信息

### 编辑`config/routes.rb`，增加路由信息

```rb
  resources :sessions, only: [:create]
```

- 测试失败：`The action 'create' could not be found for SessionsController`
- 原因：没有定义`create`方法

### 编辑`controllers/sessions_controller`，增加`create`方法

```rb
  def create
  end
```

- 测试失败：`Missing template sessions/create, application/create with {:l...`
- 原因：没有`sessions/create`模板

### 编辑`controllers/sessions_controller`，修改`create`方法内容

```rb
  def create
    session[:user_name] = params[:user_name]
    redirect_to root_path
  end
```

- 测试通过！

## 确认用户故事，增加spec

### 编辑`spec/features/guest_can_sign_in_spec.rb`

```rb
  scenario '应该显示用户名称' do
    visit "/sign_in"

    fill_in "user_name", with: @user.name
    click_button "登录"

    page.should have_content @user.name    
  end
```

- 测试失败：`expected there to be text "test_user" in "社区 会员 knowa...`
- 原因：还没有写显示用户名称的代码

### 编辑`layouts/application.html.haml`

把第22行knowang的部分修改为实际代码

```rb
            = current_user.name
```

- 测试失败：`undefined local variable or methodcurrent_user' for #<...``
- 原因：没有定义`current_user`方法

### 编辑`controllers/application_controller.rb`，增加`current_user`的定义

```rb
class ApplicationController < ActionController::Base
  protect_from_forgery

  helper_method :current_user

  private
  def current_user
    user = User.new
    user = User.find_by_name(session[:user_name]) if session[:user_name]
    user
  end
end
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f7 --no-ff
git branch -d f7
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -8](http://ruby-china.org/topics/7781)

## 用户故事

访问者希望看到帖子的节点

- 在帖子列表中显示帖子名称

## 环境准备

```bash
git checkout -b f2
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_see_node_name_spec.rb`

```rb
# coding: utf-8
feature '访问者希望看到帖子的节点名称' do
  background do
    Topic.delete_all

    Topic.create title: "DHH 的公开课"
    Topic.create title: "Rails3 中 compass 的 IE 使用问题"
    Topic.create title: "这周二上海搞Ruby Tuesday么？"
  end

  scenario do
    visit '/topics'

    page.should have_content '瞎扯淡'
  end
end
```

- 测试失败：`expected there to be text "瞎扯淡" in "社区 会员 knowan...`
- 原因：还没有实现的代码

### 编辑topics/index.html.haml

把这一行

```rb
= link_to "分享", nil, class: "node"
```

修改为

```rb
= link_to topic.node.name, nil, class: "node"
```

- 测试失败：`undefined methodnode' for #Topic:0x007fa4bc0d96d0``
- 原因：没有`node`这个方法

### 编辑`model/topic.rb`

增加

```rb
  belongs_to :node
```

- 测试失败：`undefined methodname' for nil:NilClass``
- 原因：`node`没有值

### 生成`Node`这个`model`，在`rails console`中执行

```rb
[5] pry(main)> generate "model node name:string"
      invoke  active_record
      create    db/migrate/20121231023808_create_nodes.rb
      create    app/models/node.rb
=> "Completed"
[6] pry(main)>
```

### 增加`node_id`。在`rails console`中执行

```rb
[13] pry(main)> generate "migration AddNodeIdToTopics node_id:integer"
      invoke  active_record
      create    db/migrate/20121231030435_add_node_id_to_topics.rb
=> "Completed"
[14] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

### 修改测试用例

```rb
# coding: utf-8
feature '访问者希望看到帖子的节点名称' do
  background do
    Topic.delete_all
    Node.delete_all

    node = Node.create name: "瞎扯淡"

    Topic.create title: "DHH 的公开课", node: node
    Topic.create title: "Rails3 中 compass 的 IE 使用问题", node: node
    Topic.create title: "这周二上海搞Ruby Tuesday么？", node: node
  end

  scenario '访问/topics, 应该显示所有帖子的节点名称' do
    visit '/topics'

    Topic.all.each do |topic|
      page.should have_content topic.node.name
    end
  end
end
```

- 测试失败：`Can't mass-assign protected attributes: node`
- 原因：没有对`node`设置访问属性

### 修改`model/node.rb`，增加`node`的属性声明

```rb
  attr_accessible :name, :node
```

- 当前测试成功，但前一个测试失败
- 原因：前一个测试用例中没有设置`node`信息

### 修改测试用例`spec/features/guest_can_see_all_topics_spec.rb`，增加`Node`的赋值

```rb
    Node.delete_all

    node = Node.create name: "瞎扯淡"

    Topic.create title: "DHH 的公开课", node: node
    Topic.create title: "Rails3 中 compass 的 IE 使用问题", node: node
    Topic.create title: "这周二上海搞Ruby Tuesday么？", node: node
```

- 测试全部通过

## 提交代码

```bash
git add .
git commit 
git checkout dev
git merge f2 --no-ff
git branch -d f2
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -3](/2013/01/06/ruby-china-clone-3)
