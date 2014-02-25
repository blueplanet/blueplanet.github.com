---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -6"
date: 2013-01-06 20:06
comments: true
tags: Rails, RSpec, BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -5](/2013/01/06/ruby-china-clone-5)


## 用户故事

访问者希望注册用户

- 通过用户名和Email注册

## 环境准备

```bash
git checkout -b f6
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_sign_up_spec.rb`

```rb
# coding: utf-8
feature '访问者希望注册用户' do
  scenario '访问/sign_up, 应该显示注册用页面' do
    visit '/sign_up'

    page.should have_field "user_name"
    page.should have_field "user_email"
  end
end
```

- 测试失败：`No route matches [GET] "/sign_up"`
- 原因：没有`sign_up`的路由信息

### 编辑`config/routes.rb`，增加下列路由

```rb
  match 'sign_up', to: "users#new"
```

- 测试失败：`The action 'new' could not be found for UsersController`
- 原因：`users_controller`没有`new`方法

### 编辑`controllers/users_controlelr.rb`，增加`new`方法

```rb
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end

  def new
  end
end
```

- 测试失败：`Missing template users/new, application/new with {:locale=>[:e...`
- 原因：没有`new`模板

### 新建`users/new.html.haml`模板

拷贝`ui/signup.html.haml`内容至当前模板

- 测试失败：`expected to find field "name" but there were no matches`
- 原因：模板不是实际的内容，没有对应的字段

### 编辑`users/new.html.haml`模板

```rb
= render 'shared/breadcrumb', links: [["Home", ""]]

%section#new_user.box
  %h1 注册新用户
  = form_for @user, html: {class: "form-horizontal"} do |f|
    .control-group
      = f.label "用户名", class: "control-label"
      .controls
        = f.text_field :name
      = f.label "Email", class: "control-label"
      .controls
        = f.text_field :email, placeholder: "foo@bar.com"
    .submit-button
      = f.submit "提交注册信息", class: "btn btn-primary"

%section#sidebar
  %section#has_account.box
    %h2 已经有帐号了？
    = link_to "登录"
```

- 测试失败：`undefined methodmodel_name' for NilClass:Class``
- 原因：没有设置`@user`变量

### 编辑`controllers/users_controller.rb`，在`new`方法中增加对`@user`赋值

```rb
  def new
    @user = User.new
  end
```

- 测试失败：`undefined methodusers_path' for #<#<Class:0x007fd32d...``
- 原因：没有对`users#create`的路由信息

### 编辑`config/routes.rb`，增加`users#create`的路由信息

```rb
  resources :users, only: [:show, :create]
```

- 测试失败：`undefined methodemail' for #<User id: nil, nam...``
- 原因：`user`中没有定义`email`字段

### 在`rails consle`中执行

```rb
[9] pry(main)> generate "migration AddEmailToUsers email:string"
      invoke  active_record
      create    db/migrate/20130102003510_add_email_to_users.rb
=> "Completed"
[10] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

- 测试通过！

## 确认用户故事

###增加测试用例

```
# coding: utf-8
feature '访问者希望注册用户' do
  scenario '访问/sign_up, 应该显示注册用页面' do
    visit '/sign_up'

    page.should have_field "user_name"
    page.should have_field "user_email"
  end

  scenario '输入用户名和Email，点击"提交注册信息"按钮，应该显示首页' do
    visit '/sign_up'

    fill_in "user_name", with: "test_user"
    fill_in "user_email", with: "test@test.com"

    click_button "提交注册信息"

    current_path.should == root_path
  end
end
```

- 测试失败：`The action 'create' could not be found for UsersController`
- 原因：没有定义`create`方法

### 编辑`controllers/users_controller.rb`，增加`create`方法

```rb
  def create
  end
```

- 测试失败：`Missing template users/create, application/create with {:loca...`
- 原因：没有`create`模板

### 编辑`create`方法

```rb
  def create
    User.create(params[:user])
    redirect_to root_path
  end
```

- 测试失败：`Can't mass-assign protected attributes: email`
- 原因：没有`email`的属性声明

### 编辑`model/user.rb`，增加`email`属性声明

```rb
  attr_accessible :name, :email
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f6 --no-ff
git branch -d f6
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -7](/2013/01/06/ruby-china-clone-7)
