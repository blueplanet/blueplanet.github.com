---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -5"
date: 2013-01-06 19:58
comments: true
tags: Rails, RSpec, BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -4](/2013/01/06/ruby-china-clone-4)

## 用户故事

访问者希望看到用户的信息

- 用户名称、创建日期

## 环境准备

```bash
git checkout -b f5
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/guest_can_see_user_info_spec.rb`

```rb
# coding: utf-8
feature '访问者希望看到用户的信息' do
  scenario '访问/users/:id, 应该显示用户信息' do
    visit "/users/1"

    page.should have_content "test_user"
  end
end
```

- 测试失败：`No route matches [GET] "/users/1"`
- 原因：没有`/users/1`对应的路由信息

### 编辑`config/routes.rb`，增加路由信息

```rb
  resources :users, only: [:show]
```

- 测试失败：`uninitialized constant UsersController`
- 原因：没有`UsersController`

### 在`rails console`中执行

```rb
[7] pry(main)> generate "controller users"
      create  app/controllers/users_controller.rb
      invoke  haml
      create    app/views/users
=> "Completed"
[8] pry(main)>
```

- 测试失败：`The action 'show' could not be found for UsersController`
- 原因：没有`show`方法

### 编辑`controllers/users_controller`，增加`show`方法

```rb
class UsersController < ApplicationController
  def show
  end
end
```

- 测试失败：`Missing template users/show, application/show with {:loc...`
- 原因：没有`show`模板

### 新建`users/show.html.haml`

拷贝`ui/user.html`内容至当前模板

```rb
%section#user.box
  %h1 个人信息
  %img(src="#{gravatar_url('joe@example.com')}")
  %dl
    %dt.span1 ID:
    %dd
      %strong chucai
    %dt.span1 Since:
    %dd 2012年2月12日
```

**注意：当前用户故事只考虑基本用户信息，最新帖子和最新回复以后的步骤中会增加**

- 测试失败：`expected there to be text "test_user" in "社区 会员 knowa...`
- 原因：页面还是写死的数据

### 修改模板`users/show.html.haml`

```rb
%section#user.box
  %h1 个人信息
  %img(src="#{gravatar_url('joe@example.com')}")
  %dl
    %dt.span1 ID:
    %dd
      %strong= @user.name
    %dt.span1 Since:
    %dd= @user.created_at
```

- 测试失败：`undefined methodname' for nil:NilClass``
- 原因：没有对`@user`变量赋值

### 编辑`controllers/users_controller.rb`

```rb
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end
end
```

- 测试失败：`uninitialized constant UsersController::User`
- 原因：没有`user`这个`model`

### 在`rails console`中执行

```rb
[8] pry(main)> generate "model user name:string"
      invoke  active_record
      create    db/migrate/20130101221838_create_users.rb
      create    app/models/user.rb
=> "Completed"
[9] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

- 测试失败：`Couldn't find User with id=1`
- 原因：测试用例中使用的是写死的数据

### 修改测试用例，增加`user`部分

```rb
# coding: utf-8
feature '访问者希望看到用户的信息' do
  background do
    @user = User.create name: "test_user"
  end

  scenario '访问/users/:id, 应该显示用户信息' do
    visit "/users/#{@user.id}"

    page.should have_content @user.name
    page.should have_content @user.created_at.to_s
  end
end
```

- 测试通过！

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f5 --no-ff
git branch -d f5
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -6](/2013/01/06/ruby-china-clone-6)
