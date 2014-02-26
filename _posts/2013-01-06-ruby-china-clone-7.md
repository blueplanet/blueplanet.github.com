---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -7"
date: 2013-01-06 20:59
comments: true
tags: Rails RSpec BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -6](/2013/01/06/ruby-china-clone-6)

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

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -8](/2013/01/06/ruby-china-clone-8)
