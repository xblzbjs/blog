+++
title = "Git｜分支规范"
date = "2021-06-19"
author = "xblzbjs"
tags = [
    "git",
]
+++

工作中为规范开发，保持代码提交记录以及git分支结构和commit message整体清晰易读、方便维护，公司一般都会根据不同的项目规格置顶不同的Git分支规范。

## 命名规范

在[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)文中有一图展示了常见的分支管理规范

![无法显示图片](/img/git-model.png)

从上图（自右向左）分别为

- `master`：主分支。负责管理发布的状态，提交时使用标签记录发布版本号。
- `hotfix-修复的问题`：紧急修复分支。发布的产品需要紧急修正时，就直接从master主分支切出一个分支紧急修改，通常会加上`hotfix-`前缀，改好后再合并到master分支和develop分支上。
- `release-版本`：预发布分支。开发人员在develop分支开发到了感觉可以发布的状态会创建release分支，为release做最后的bug修正。当感觉release分支上没有问题时，就会将release分支合并到master分支上正式发布。通常一个项目会有多个release分支，例如`release-1.0`，`release-2.0`等
- `develop`：开发分支。开发人员的日常开发分支。
- `feature-功能`：功能分支。从develop分支分叉出来的针对新功能的开发分支，通常会有多个feature分支并行开发。完成功能的开发后就把分支合并回develop分支（通常来说，feature分支大多会保留，并不会merge之后就删除原分支）

## 日志规范

> 在一个团队协作的项目中，开发人员需要经常提交一些代码去修复bug或者实现新的feature。而项目中的文件和实现什么功能、解决什么问题都会渐渐淡忘，最后需要浪费时间去阅读代码。但是好的日志规范commit messages编写有帮助到我们，它也反映了一个开发人员是否是良好的协作者。

**编写良好的Commit messages的好处：**

- 加快review的流程
- 编写良好的版本发布日志
- 让之后的维护者了解代码里出现特定变化和feature被添加的原因

当前业界应用的比较广泛的是 [Angular Git Commit Template](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-guidelines)

具体格式为:

```
<type>(<scope>): <subject>

<body>

<footer>
复制代码
```

- type: 本次 commit 的类型
  - feat: 添加新特性
  - fix: 修复bug
  - docs: 仅仅修改了文档
  - style: 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑
  - refactor: 代码重构，没有加新功能或者修复bug
  - perf: 增加代码进行性能测试
  - test: 增加测试用例
  - chore: 改变构建流程、或者增加依赖库、工具等
- scope: 本次 commit 波及的范围，例如Compiler, ElementInjector等。
- subject: 简明扼要的阐述下本次 commit 的主旨。
- body: 包括改变的动机，并将其与以前的行为进行对比。
- footer: 描述下与之关联的 issue 或 break change。

### Commit messages格式要求

```
# 标题行：50个字符以内，描述主要变更内容
#
# 主体内容：更详细的说明文本，建议72个字符以内。 需要描述的信息包括:
#
# * 为什么这个变更是必须的? 它可能是用来修复一个bug，增加一个feature，提升性能、可靠性、稳定性等等
# * 他如何解决这个问题? 具体描述解决问题的步骤
# * 是否存在副作用、风险? 
#
# 如果需要的化可以添加一个链接到issue地址或者其它文档
```

## 参考链接

### 其他Commit Messages 模板

- [Using Git Commit Message Templates to Write Better Commit Messages](https://gist.github.com/lisawolderiksen/a7b99d94c92c6671181611be1641c733)
- [another git commit message template](https://gist.github.com/zakkak/7e06725ebd1336bfebebe254de3de825)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)