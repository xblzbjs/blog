+++
title = "Git｜工作流设计"
date = "2021-06-19"
author = "xblzbjs"
tags = [
    "Git",
]
+++

## 前言

现代软件开发讲究效率和质量，大多依赖于多团队间的协作来实现。不同的软件团队规模也不同：十人、百人甚至千人团队。如果软件架构没有良好的拆分，很有可能出现几百人在一个代码仓库里面工作的情况，而团队也需要根据软件的发布节奏来完成代码提交、审核、集成、测试等工作。因此，如何**根据团队规模设计一个合理的工作流**是编码之前就要思考的。工作中为规范开发，保持代码提交记录以及 git 分支结构和 commit message 整体清晰易读、方便维护，公司一般都会根据不同的项目规格置顶不同的 Git 工作流。

## 常见的工作流

在使用 Git 开发时，有 4 种常用的工作流：集中式工作流（Centralized Workflow）、功能分支工作流（Feature Branch Workflow）、Gitflow 工作流和 Forking 工作流。

### 集中式工作流（Centralized Workflow）

开发团队成员在本地都有一份远程仓库的拷贝：本地仓库。在本地的 master 分支开发完代码之后，将修改后的代码 commit 到远程仓库，如果有冲突就先解决本地的冲突再提交。示例图如下：

![无法显示图片](/img/git/centralized-workflow.png)

#### 举个例子

1. 克隆远程仓库到本地

```bash
git clone ssh://user@host/path/to/repo.git
```

2. 本地开发完后，提交代码

```bash
git status	# 查看仓库状态
git add <some file> #
git commit -m "..."
```

#### 优点

- 代码集成频率快
- 特性相对独立清晰

#### 缺点

- 不同开发人员的提交日志混杂在一起，难以定位问题
- 如果同时开发多个功能，不同功能同时往 master 分支合并，代码之间也会相互影响，从而产生代码冲突
- 对开发者的提交代码质量要求高

#### 适合场景

- 团队人数少
- 开发不频繁
- 不需要同时维护多个版本的小项目
- 自动化验收较为成熟的公司

### 功能分支工作流（Feature Branch Workflow）

在开发新功能时，基于主分支新建一个功能分支，在功能分支上进行开发，而不是直接在本地的主分支开发，开发完成之后合并到主分支。示例图如下：

![无法显示图片](/img/git/feature-branch-workflow.png)

需要注意的是，在合并到主分支时，可以根据需要提交 PR（pull request），而不是直接将代码 merge 到主分支。PR 流程可以把分支代码提供给团队其他开发人员进行 CR（Code Review），还可以在 PR 页面讨论。这样代码更加健壮，质量更高并且提供了一个历史回顾的途径。

#### 举个例子

1. 基于主分支新建一个功能分支

```bash
git checkout -b feature-user
```

2. 在功能分支上进行代码开发，开发完成后 commit 到功能分支

```bash
git add <some file>
git commit -m "add some file"
```

3. 将本地功能分支代码 push 到远程仓库

```bash
git push origin feature-user
```

4. （可选）在远程仓库上创建 PR
5. （可选）代码管理员或其他同时收到 PR 后，CR 代码
6. **Merge pull request**将功能分支合并到主分支

#### 优点

- 分支开发相对独立，不会因为并行导致互相干扰
- 以功能为粒度进行发布的模式

#### 缺点

- 考研团队功能拆分的能力，**一般来说分支存活的周期一般不要超过一周**
- 功能分支的命名规范要求很高。由于大量功能分支的拉出，整个代码仓库会显得非常乱。面对一大堆分支，谁也说不清到底哪个还活着，哪个已经没用了。所以，如果能够跟**变更管理系统**打通，**自动化创建分支**就最好了。

#### 适合场景

- 开发团队相对固定
- 中小规模的项目
- 已经拥有强大的测试环境和自动化的特性管理工具作支撑的团队

### Gitflow 工作流（Gitflow Workflow）

非开源项目中最常用到的工作流。

在[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)文中有一图形象展示了该工作流

![无法显示图片](/img/git/gitflow-workflow.png)

从上图（自右向左）分别为

- `master`：主分支。负责管理发布的状态，提交时使用标签记录发布版本号。
- `hotfix-修复的问题`：紧急修复分支。发布的产品需要紧急修正时，就直接从 master 主分支切出一个分支紧急修改，通常会加上`hotfix-`前缀，改好后再合并到 master 分支和 develop 分支上。
- `release-版本`：预发布分支。开发人员在 develop 分支开发到了感觉可以发布的状态会创建 release 分支，为 release 做最后的 bug 修正。当感觉 release 分支上没有问题时，就会将 release 分支合并到 master 分支上正式发布。通常一个项目会有多个 release 分支，例如`release-1.0`，`release-2.0`等
- `develop`：开发分支。开发人员的日常开发分支。
- `feature-功能`：功能分支。从 develop 分支分叉出来的针对新功能的开发分支，通常会有多个 feature 分支并行开发。完成功能的开发后就把分支合并回 develop 分支（通常来说，feature 分支大多会保留，并不会 merge 之后就删除原分支）

#### 举个例子

1. 基于 develop 分支，新建一个功能分支

```bash
git checkout -b feature-<your name> # or feature/<your name>
```

2. 在功能分支上开发
3. 紧急修复 Bug

假设我们正处于新功能的开发中，突然线上代码发现了一个 Bug，我们要立即停止手上的工作，修复线上的 Bug

```bash
git stash # 1. 开发工作只完成了一半，还不想提交，可以临时保存修改至堆栈区
git checkout -b hotfix/print-error master # 2. 从 master 建立 hotfix 分支
vi main.go # 3. 修复 bug，callmainfunction -> call main function
git commit -a -m 'fix print message error bug' # 4. 提交修复
git checkout develop # 5. 切换到 develop 分支
git merge --no-ff hotfix/print-error # 6. 把 hotfix 分支合并到 develop 分支
git checkout master # 7. 切换到 master 分支
git merge --no-ff hotfix/print-error # 8. 把 hotfix 分支合并到 master
git tag -a v0.9.1 -m "fix log bug" # 9. master 分支打 tag
go build -v . # 10. 编译代码，并将编译好的二进制更新到生产环境
git branch -d hotfix/print-error # 11. 修复好后，删除 hotfix/xxx 分支
git checkout feature/print-hello-world # 12. 切换到开发分支下
git merge --no-ff develop # 13. 因为 develop 有更新，这里最好同步更新下
git stash pop # 14. 恢复到修复前的工作状态
```

4. 继续开发，完成后提交代码到功能分支上
5. 在功能分支上做 CR
6. CR 通过后，由代码仓库 matainer 将功能分支合并到 develop 分支上

```bash
git checkout develop
git merge --no-ff feature-<your name>
```

7. 基于 develop 分支，创建 release 分支，测试代码

```bash
git checkout -b release-<版本号> develop
```

8. 测试失败的话，直接在 release 分支上修改代码，修改完成后，提交并编译部署。

```bash
git commit -a -m "fix bug"
go build -v .
```

9. 测试通过后，将功能分支合并到主分支和 develop 分支

```bash
git checkout develop
git merge --no-ff release-<版本号>
git checkout master
git merge --no-ff release-<版本号>
git tag -a v1.0.0 -m "add <your feature description>" # master 分支打 tag
```

10. 删除原本的功能分支，也可以选择性删除对应的 release 分支

#### 优点

- 每个分支分工明确，可并行开发多个功能

#### 缺点

- 有一定的上手难度

#### 适合场景

- 迭代速度快的大型项目

### Forking 工作流（Forking Workflow）

开源项目中最常用到的工作流，例如 Kubernetes、Docker 等项目。fork 操作是在个人远程仓库新建一份目标远程仓库的副本，比如在 GitHub 上操作时，在项目的主页点击 fork 按钮（页面右上角），即可拷贝该目标远程仓库。示例图如下：

![无法显示图片](/img/git/forking-workflow.png)

#### 举个例子

以 Github 为例

1. Fork 远程仓库到自己的账号下
2. 克隆 fork 的仓库到本地
3. 创建功能分支
4. 提交 commit
5. push 功能分支到个人远程仓库
6. 个人远程仓库页面创建 pull request

#### 优点

- 项目远程仓库和开发者远程仓库完全独立，避免授予开发者项目远程仓库的权限，从而提高安全性
- 任意开发者都可以参与项目的开发

#### 缺点

- 迭代速度慢
- 开发者沟通困难

#### 适用场景

- 开源项目
- 开发者不固定，可能是任意一个能访问到项目的开发者
- 有项目衍生版的需求

## 总结

- 集中式工作流：开发者直接在本地 master 分支开发代码，开发完成后 push 到远端仓库 master 分支。
- 功能分支工作流：开发者基于 master 分支创建一个新分支，在新分支进行开发，开发完成后合并到远端仓库 master 分支。
- Git Flow 工作流：Git Flow 工作流为不同的分支分配一个明确的角色，并定义分支之间什么时候、如何进行交互，比较适合大型项目的开发。
- Forking 工作流：开发者先 fork 项目到个人仓库，在个人仓库完成开发后，提交 pull request 到目标远程仓库，远程仓库 review 后，合并 pull request 到 master 分支。

总的来说，在选择工作流时，推荐如下：

- 非开源项目采用 Git Flow 工作流。
- 开源项目采用 Forking 工作流

不同类型、规模、行业的软件项目采用的 Git 工作流可能都不尽相同，同时，发布频率、软件架构、基础设施能力、人员能力水平等因素也在制约着工作流的应用效果。所以，很难说有一种通用的工作流可以满足所有场景的需求，找到适合当前团队的工作流是最重要的。

## 附录

### Git Commit 规范

> 在一个团队协作的项目中，开发人员需要经常提交一些代码去修复 bug 或者实现新的 feature。而项目中的文件和实现什么功能、解决什么问题都会渐渐淡忘，最后需要浪费时间去阅读代码。但是好的日志规范 commit messages 编写有帮助到我们，它也反映了一个开发人员是否是良好的协作者。

**编写良好的 Commit messages 的好处：**

- 加快 review 的流程
- 编写良好的版本发布日志
- 让之后的维护者了解代码里出现特定变化和 feature 被添加的原因

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
  - fix: 修复 bug
  - docs: 仅仅修改了文档
  - style: 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑
  - refactor: 代码重构，没有加新功能或者修复 bug
  - perf: 增加代码进行性能测试
  - test: 增加测试用例
  - chore: 改变构建流程、或者增加依赖库、工具等
- scope: 本次 commit 波及的范围，例如 Compiler, ElementInjector 等。
- subject: 简明扼要的阐述下本次 commit 的主旨。
- body: 包括改变的动机，并将其与以前的行为进行对比。
- footer: 描述下与之关联的 issue 或 break change。

#### Commit messages 格式要求

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

#### 其他 Commit Messages 模板

- [Using Git Commit Message Templates to Write Better Commit Messages](https://gist.github.com/lisawolderiksen/a7b99d94c92c6671181611be1641c733)
- [another git commit message template](https://gist.github.com/zakkak/7e06725ebd1336bfebebe254de3de825)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
