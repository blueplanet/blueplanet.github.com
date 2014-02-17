---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -8"
date: 2013-01-06 21:04
comments: true
categories: Rails, RSpec, BDD
---
[目录](/blog/2013/01/06/ruby-china-clone-cover)
上一步：[使用 RSpec+Capybara 简单BDD入门 -7](/blog/2013/01/06/ruby-china-clone-7)

## 用户故事

用户希望公开帖子

输入下列项目
- 节点
- 标题
- 内容
点击保存按钮后，显示帖子列表

- 帖子列表显示帖子创建者信息
- 在用户信息页面显示最新公开的帖子列表

## 环境准备

```bash
git checkout -b f8
rails c # console
rails s # server
```

## 步骤

### 新建文件`spec/features/user_can_publish_topic_spec.rb`

```rb
# coding: utf-8
feature '用户希望公开帖子' do
  background do
    @user = User.create name: 'test_user'
  end

  before do
    visit '/sign_in'

    fill_in 'user_name', with: @user.name
    click_button "登录"
  end

  scenario '访问/topics/new，应该显示新建帖子页面' do
    click_link "发布新帖"

    current_path.should == new_topic_path
    page.should have_content "新建帖子"
  end
end
```

- 测试失败：`undefined local variable or methodnew_topic_path' for #<RSpec:...``
- 原因：没有`new_topic`的路由信息

### 编辑`config/routes.rb`，增加路由信息

```rb
  resources :topics, only: [:index, :show, :new]
```

- 测试失败：`expected there to be text "新建帖子" in "社区 会员...`
- 原因：“新建帖子”的链接还没有加上

### 编辑`topics/index.html.haml`

修改第10行，增加链接

```rb
    = link_to "发布新帖", new_topic_path, class: "btn btn-success"
```

- 测试失败：`The action 'new' could not be found for TopicsController`
- 原因：没有定义`new`方法

### 编辑`controllers/topics_controller.rb`，增加`new`方法

```rb
  def new
  end
```

- 测试失败：`Missing template topics/new, application/new with {:locale=>...`
- 原因：没有`topics/new`模板

### 新建`topics/new.html.haml`模板

拷贝`ui/new_topic.html`内容至当前文件

- 测试通过！

## 确认用户故事，增加`spec`

```rb
# coding: utf-8
feature '用户希望公开帖子' do
  background do
    @user = User.create name: 'test_user'

    Node.create name: "Ruby"
    Node.create name: "Rails"
    Node.create name: "XiaCheDan"
  end

  before do
    visit '/sign_in'

    fill_in 'user_name', with: @user.name
    click_button "登录"
  end

  scenario '访问/topics/new，应该显示新建帖子页面' do
    click_link "发布新帖"

    page.should have_content "新建帖子"
    page.should have_select "topic_node", options: Node.all.map { |node| node.name }
    page.should have_field 'topic_title'
    page.should have_field 'topic_content'
  end
end
```

- 测试失败：`expected to find select box "topic_node" but there wer...`
- 原因：模板还是写死的数据

### 编辑`topics/new.html.haml`

```rb
= render 'shared/breadcrumb', links: [["Home", ""], ["社区", ""], ["发帖", ""]]

%section#new_topic.box
  %h1 新建帖子
  = form_for @topic, html: {class: "form-horizontal"} do |f|
    .control-group
      = f.label "标题", class: "control-label"
      .controls
        = f.select :node_id, Node.all.map{ |node| [node.name, node.id] }
        %select
          %option(value="Ruby") Ruby
          %option(value="Rails") Rails
          %option(value="XiaCheDan") XiaCheDan
        = f.text_field :title
    .control-group
      = f.label "正文"
      .controls
        = f.text_area :content, rows: 20
    .submit-button
      = f.submit "保存", class: "btn btn-primary"
      = link_to "取消"

%section#sidebar
  %section#new_post_note.box
    %h2 发帖说明
    %p 没啥可说的，发就是了
```

- 测试失败：`undefined methodmodel_name' for NilClass:Class``
- 原因：没有对`@topic`变量赋值

### 编辑`controllers/topics_controller.rb`

```rb
  def new
    @topic = Topic.new
  end
```

- 测试通过！

## 确认用户故事，增加spec

```rb
# coding: utf-8
feature '用户希望公开帖子' do
  background do
    @user = User.create name: 'test_user'

    @ruby_node = Node.create name: "Ruby"
    Node.create name: "Rails"
.....
  scenario '输入节点、标题、内容，点击"保存"按钮，应该正常提交并显示帖子列表' do
    click_link "发布新帖"

    select @ruby_node.name, from: 'topic_node_id'
    fill_in 'topic_title', with: 'new topic title'
    fill_in 'topic_content', with: 'new topic content'

    click_button "保存"

    current_path.should == root_path
    page.should have_content 'new topic title'
  end
end
```

- 测试失败：`No route matches [POST] "/topics"`
- 原因：没有`topics#create`的路由信息

### 编辑`config/routes.rb`，增加路由信息

```rb
  resources :topics, only: [:index, :show, :new, :create]
```

- 测试失败：`The action 'create' could not be found for TopicsController`
- 原因：没有定义`create`方法

### 编辑`controllers/topics_controller.rb`

```rb
  def create
  end
```

- 测试失败：`Missing template topics/create, application/create with {:loca...`
- 原因：没有`create`模板。

### 编辑`controllers/topics_controller.rb`

```rb
  def create
    Topic.create(params[:topic])
    redirect_to root_path
  end
```

- 测试失败：`Can't mass-assign protected attributes: node_id`
- 原因：没有对`node_id`的访问属性声明

### 编辑`model/topic.rb`，增加属性声明

```rb
  attr_accessible :title, :node, :content, :node_id
```

- 测试通过！！

## 重构

spec文件中，`click_link "发布新帖"`可以移到`before`里面

```rb
  before do
    visit '/sign_in'

    fill_in 'user_name', with: @user.name
    click_button "登录"

    click_link "发布新帖"
  end

  scenario '访问/topics/new，应该显示新建帖子页面' do
    page.should have_content "新建帖子"
    page.should have_select "topic_node_id", options: Node.all.map { |node| node.name }
    page.should have_field 'topic_title'
    page.should have_field 'topic_content'
  end

  scenario '输入节点、标题、内容，点击"保存"按钮，应该正常提交并显示帖子列表' do
    select @ruby_node.name, from: 'topic_node_id'
    fill_in 'topic_title', with: 'new topic title'
    fill_in 'topic_content', with: 'new topic content'

    click_button "保存"

    current_path.should == root_path
    page.should have_content 'new topic title'
  end
```

- 测试应该仍然全部通过

## 确认用户故事，修改spec

### 编辑`spec/features/topics_spec.rb`，增加帖子创建者的验证部分

- 第二行的feature描述错了，也顺便修改一下 :)
- 顺便把重复的部分重构一下 :)

```rb
# coding: utf-8
feature '访问者希望看到所有帖子' do
  background do
    node = Node.create name: "瞎扯淡"
    @user = User.create name: 'test_user'

    Topic.create title: "DHH 的公开课", node: node
    Topic.create title: "Rails3 中 compass 的 IE 使用问题", node: node
    Topic.create title: "这周二上海搞Ruby Tuesday么？", node: node
  end

  context "访问/topics" do
    before do
      visit '/topics'
    end

    scenario '应该显示所有帖子' do
      Topic.all.each { |topic| page.should have_content topic.title }

      page.should have_content "published less than a minute ago"
    end

    scenario '应该显示帖子的节点名称' do
      Topic.all.each { |topic| page.should have_content topic.node.name } 
    end

    scenario '应该显示帖子创建者名称' do
      page.should have_content @user.name
    end
  end
end
```

- 测试失败：`expected there to be text "test_user" in "社区 会员...`
- 原因：还没有显示创建者名称

### 编辑`topics/_topics.html.haml`

修改第12行

```rb
          = link_to "knwang", nil, class: "user_link"
```

为

```rb
          = link_to topic.author.name, topic.author, class: "user_link"
```

- 测试失败：`undefined methodanthor' for #Topic:0x007fd32d943a60``
- 原因：没有定义`author`方法

### 编辑`models/topic.rb`，增加`author`的声明

```rb
  belongs_to :anthor, class_name: "User"
```

### 在`rails console`中执行

```rb
[20] pry(main)> generate "migration AddAuthorIdToTopics author_id:integer"
      invoke  active_record
      create    db/migrate/20130102235735_add_author_id_to_topics.rb
=> "Completed"
[21] pry(main)>
```

### 执行数据库升级

```bash
bundle exec rake db:migrate
bundle exec rake db:test:prepare
```

### 编辑`spec`，修改`Topic`，增加`author`的赋值部分

```rb
    Topic.create title: "DHH 的公开课", node: node, author: @user
    Topic.create title: "Rails3 中 compass 的 IE 使用问题", node: node, author: @user
    Topic.create title: "这周二上海搞Ruby Tuesday么？", node: node, author: @user
```

- 测试失败：`Can't mass-assign protected attributes: author`
- 原因：没有设置`author`的访问属性

### 编辑`models/topic.rb`，增加访问属性声明

```rb
  attr_accessible :title, :node, :content, :node_id, :author
```

- 部分测试成功，但有其他的测试失败

#### 编辑`spec/features/user_can_publish_topic_spec.rb`

- 测试失败原因：公开帖子的时候没有设置`author`

```rb
  def create
    topic = Topic.new params[:topic]
    topic.author = current_user
    topic.save

    redirect_to root_path
  end
```

- 测试成功

#### 编辑`spec/features/nodes_spec.rb`

- 测试失败原因：数据准备部分没有设置`author`

```rb
  background do
    user = User.create name: 'test_user'

    @node1 = Node.create name: "Ruby Node", description: "Ruby是一门优美的语言"
    @node2 = Node.create name: "Rails Node", description: "Rails是一个快速WEB开发框架"

    2.times.map.with_index { |i| Topic.create title: "topic #{i}", node: @node1, author: user}
    3.times.map.with_index { |i| Topic.create title: "topic #{i + 10}", node: @node2, author: user}
  end
```

- 测试通过！

## 再次确认用户故事，增加spec

### 编辑`spec/features/guest_can_see_user_info_spec.rb`，增加最新帖子的验证部分

```rb
# coding: utf-8
feature '访问者希望看到用户的信息' do
  background do
    @user = User.create name: "test_user"

    5.times.map.with_index { |i| Topic.create title: "topic #{i}", author: @user }
  end

  scenario '访问/users/:id, 应该显示用户信息' do
    visit "/users/#{@user.id}"

    page.should have_content @user.name
    page.should have_content @user.created_at.to_s

    Topic.all.each do |topic|
      page.should have_content topic.title
    end
  end
end
```

- 测试失败：`expected there to be text "topic 0" in "社区 会...`
- 原因：还没有实现最新回复的部分

### 编辑`users/show.html.haml`

拷贝`ui/user.html.haml`中最新回复的部分至当前模板最下方

```rb
%section#user_recent_topics.box
  %section#user_recent_topics_banner.box.info-box
    %span 最近发布的帖子
  %section#user_recent_topics_content.box
    %ul
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
```

修改为

```rb
%section#user_recent_topics.box
  %section#user_recent_topics_banner.box.info-box
    %span 最近发布的帖子
  %section#user_recent_topics_content.box
    %ul
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
      %li
        = link_to "Rails", nil, class: "node"
        = link_to "BDD开发模式:使用Rspec开发Rails Project的标准流程是什么?"
        %span 3 days ago
```

- 测试失败：`undefined methodtopics' for #User:0x007fc0f7305830...``
- 原因：`user`还没有和`topic`关联

### 编辑`models/user.rb`，增加关联

```rb
  has_many :topics, :class_name => "Topic", :foreign_key => "author_id"
```

- 测试通过！

## 重构

### 编辑 `spec/featurs/guest_can_see_user_info_spec.rb`

- 最新发布帖子的验证部分分到单独的`scenario`
- 把重复的部分提取到`before`

```rb
# coding: utf-8
feature '访问者希望看到用户的信息' do
  background do
    @user = User.create name: "test_user"
    node = Node.create name: "ruby"

    5.times.map.with_index { |i| Topic.create title: "topic #{i}", node: node, author: @user }
  end

  before do
    visit "/users/#{@user.id}"
  end

  scenario '应该显示用户信息' do
    page.should have_content @user.name
    page.should have_content @user.created_at.to_s
  end

  scenario '应该显示用户最新发布的帖子' do
    Topic.all.each do |topic|
      page.should have_content topic.title
    end
  end
end
```

## 补充重构（可以不做）

### `guest_can_sign_in_spec.rb`中也有重复的代码可以重构

```rb
  scenario '输入用户名后点击"登录"按钮，应该正常登录' do
    visit "/sign_in"

    fill_in "user_name", with: @user.name
    click_button "登录"

    current_path.should == root_path
  end

  scenario '应该显示用户名称' do
    visit "/sign_in"

    fill_in "user_name", with: @user.name
    click_button "登录"

    page.should have_content @user.name    
  end
```

可以把重复的部分提取出来

```rb
  before do
    visit "/sign_in"

    fill_in "user_name", with: @user.name
    click_button "登录"
  end

  scenario '输入用户名后点击"登录"按钮，应该正常登录' do
    current_path.should == root_path
  end

  scenario '应该显示用户名称' do
    page.should have_content @user.name    
  end
end
```

## 完成，提交代码

```bash
git add .
git commit 
git checkout dev
git merge f8 --no-ff
git branch -d f8
```

下一步骤：[使用 RSpec+Capybara 简单BDD入门 -9](/blog/2013/01/06/ruby-china-clone-9)