---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -11"
date: 2013-01-06 21:09
comments: true
tags: Rails RSpec BDD
---
[目录](/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -10](/2013/01/06/ruby-china-clone-10)

## 用户故事

用户希望对帖子进行回复

## 环境准备

```bash
git checkout -b f11
rails c # console
rails s # server
```

## 步骤

### 编辑`spec/features/guest_can_see_topic_info_spec.rb`，增加回复form的验证部分

```rb
  scenario '应该显示回复用的form' do
    visit "/topics/#{@topic.id}"

    page.should have_field 'reply_content'
    page.should have_button '提交回复'
  end
```

- 测试失败：`expected to find field "replay_content" but there were no matches`
- 原因：没有实现的代码

### 编辑`topics/show.html.haml`

拷贝`ui/topic.html`中回复form的部分至当前模板

```rb
  %section#reply.box
    %form(action="" method="post")
      %textarea(rows="4")
      %input(type="submit" class="btn btn-primary" value="提交回复")
```

修改为

```rb
  %section#reply.box
    = form_for [@topic, Reply.new] do |f|
      = f.text_area :content, rows: 4
      = f.submit "提交回复", class: "btn btn-primary"
```

- 测试失败：`undefined methodtopic_replies_path' for #<#<Class:0x007fc0f8``
- 原因：没有设置路由信息

### 编辑`config/routes.rb`，增加嵌套的replay的路由设置

```rb
  resources :topics, only: [:index, :show, :new, :create] do
    resources :replies, only: [:create]
  end
```

- 测试通过！

### 增加提交回复的测试用例

```
  scenario '输入回复内容点击"提交回复"后，正常提交回复' do
    visit "/topics/#{@topic.id}"

    fill_in 'reply_content', with: "回复测试"
    click_button "提交回复"

    current_path.should == topic_path(@topic)
    page.should have_content "回复测试"
  end
```

- 测试失败：`uninitialized constant RepliesController`
- 原因：没有`replies`这个`controller`

### 在`rails console`中执行

```rb
[34] pry(main)> generate "controller replies"
      create  app/controllers/replies_controller.rb
      invoke  haml
      create    app/views/replies
=> "Completed"
[35] pry(main)>
```

- 测试失败：`The action 'create' could not be found for RepliesController`
- 原因：没有`create`方法

### 编辑`controllers/replies_controller.rb`，增加`create`方法

```rb
  def create
  end
```

- 测试失败：`Missing template replies/create, application/create with {:local...`
- 原因：没有`replies/create`模板

### 编辑`controllers/replies_controller.rb`，增加实际逻辑

```rb
class RepliesController < ApplicationController
  def create
    topic = Topic.find(params[:topic_id])
    reply = topic.replies.build params[:reply]
    reply.user = current_user
    reply.save

    redirect_to topic
  end
end
```

- 测试通过！

## 完成，提交代码

```rb
git add .
git commit 
git checkout dev
git merge f11 --no-ff
git branch -d f11
```

**!!全部完成!!**
