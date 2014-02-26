---
layout: post
title: "使用 RSpec+Capybara 简单BDD入门 -目录"
date: 2013-01-06 17:14
comments: true
tags: Rails RSpec BDD
---
## 前提

- 我也是个Rails初学者，文中可能会有考虑不周或者错误的地方，希望大家多指正，共同进步
- 计划是使用RSpec+Capybara写 features 测试＋简单的单元测试，并不是完全的BDD，是一个简略版本

## 缘起

- 前段时间刚看完了 The RSpec Book之后对BDD特别感兴趣，尝试了一下 cucumber 但不是很顺利就暂时放下了
- 之后有幸参加了的社区最高楼 @knwang 周四晚现场编程网上演示 面向新手的现场演示，被从外至内的理念震撼了
- @knwang 在交流时间里说过，实际项目开发的时候，会使用TDD/BDD进行开发，但基本的从外至内的理念是不变的，于是参加活动之后，先是按照 @knwang 的演示步骤自己动手做了几遍，觉得收获非常大，如果能够把RSpec加进去的话应该更完美。
- 最近又学到了RSpec写features的方法，于是决定使用 RSpec+Capybara来把 @knwang 的步骤重新做一遍，顺便把这个过程记录下来，希望能够对其他人有所帮助

## 计划

- 完全按照 @knwang 的步骤，重写一遍 Ruby China Clone
- 使用RSpec+Capybara 编写测试
- 每个步骤新开一个帖子，这个帖子作为目录。步骤完成后把新帖子链接加到这个帖子里
- 把 @knwang 的步骤翻译成了中文。由于英文水平很低，并且用户故事的写法也不是很熟练，可能看起来比较生硬，请见谅

## 步骤

1. [访问者希望看到所有帖子的列表](/2013/01/06/ruby-china-clone-1)
2. [访问者希望看到帖子的节点](/2013/01/06/ruby-china-clone-2)
3. [访问者希望看到一个节点的帖子列表](/2013/01/06/ruby-china-clone-3)
4. [访问者希望看到帖子的详细信息](/2013/01/06/ruby-china-clone-4)
5. [访问者希望看到用户的信息](/2013/01/06/ruby-china-clone-5)
6. [访问者希望注册用户](/2013/01/06/ruby-china-clone-6)
7. [访问者希望登录](/2013/01/06/ruby-china-clone-7)
8. [用户希望公开帖子](/2013/01/06/ruby-china-clone-8)
9. [用户希望看到帖子的回复列表](/2013/01/06/ruby-china-clone-9)
10. [用户希望看到最新回复的信息](/2013/01/06/ruby-china-clone-10)
11. [用户希望对帖子进行回复](/2013/01/06/ruby-china-clone-11)

## 可能出现的问题

### 无法退出。按照下面的步骤实现即可

#### 编辑`controllers/sessions_controller.rb`，增加`destroy`方法

```rb
  def destroy
    session[:user_name] = nil
    redirect_to root_path
  end
```

#### 编辑`layouts/application.html.haml`，修改第26行的链接

```rb
              = link_to "退出", sign_out_path
```

#### 编辑`conf/routes.rb`，增加路由信息

```rb
  match 'sign_out', to: "sessions#destroy"
```

### 在使用`rails server`后启动浏览器进行测试的时候没有`Node`
- 由于是示范项目，所以并没有把所有功能做完
- 可以在`rails console`里面加上几个就可以了

## 起步

- 参考我的git 库
    - gitcafe : https://gitcafe.com/blueplanet/ruby_china_clone
    - github : https://github.com/blueplanet/ruby_china_clone
- 主要有两个分支：
    - master 分支：从 @knwang 的库拷贝过来的。除了基本的框架之外还有 ui 这个 controller。相当于美工做好的设计，纯HTML加上假的数据。这个分支不打算更新
    - dev 分支：今后的开发分支。修改会提交到这个分支

## 本地开发步骤

```bash
git clone https://gitcafe.com/blueplanet/ruby_china_clone
cd ruby_china_clone
git checkout master
git checkout -b dev # 创建dev分支
```

- 启动自动测试环境

```bash
bundle install --without production # 只安装开发环境和测试环境需要的gem
bundle exec guard
```

执行之后如果出现下面的提示，说明环境没问题了

```bash
Finished in 0.00856 seconds 
0 examples, 0 failures           
Randomized with seed 26104    
Done.  
17:00:57 - INFO - Guard is now watching at ....
[1] guard(main)> 
```

以后的步骤中，新建或者保存 spec 文件之后，会自动执行对应的测试

先把坑挖好，慢慢填上！

## 感想

- 真正动手很重要
    - 实际花费时间：写帖子20小时左右，之前的练习和调查也至少是这个数字
    - 这40个小时，感觉是进步最大最快的40个小时。所以，不能只顾看书，动手才是真的！
- 实际开发很复杂
    - 由于示范的侧重点不同，实际上回避了两个比较大的问题。一是UI设计，二是用户故事的写法。
    - 实际上如果自己做项目，最先遇到的就是这两个问题。至少我是撞得够呛，呵呵。计划下个阶段摸索一下这两个方面。

最后，再次感谢 @knwang 的演示！
