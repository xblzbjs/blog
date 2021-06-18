+++
title = "Git｜基本概念"
date = "2021-03-19"
author = "xblzbjs"
tags = [
    "git",
]
+++

## git 术语

### Repository(仓库)
### Working Directory(工作区)
### Staging area(暂存区)
### Revision(修订)
### Tags(标记)
### Push(推送)
### Pull(拉取)
### Indev(索引)
### Commit(提交)
### Conflict(冲突)
### Merge(合并)
### Branch(分支)
### HEAD(头)
### Checkout(检出)
### commit(提交)
### diff(差异)
### fetch(获取)
### main/master(主干)

### merge request(合并请求)

git文件有四种状态分别是:

- Untracked: 未跟踪。此文件仅在工作区中，暂存区和版本库没有它，未进行版本控制。 新建或新增一个文件即产生一个未追踪文件。通过`git add <file>`将其加入暂存区，即成为已追踪文件。
- Unmodified: 未修改。文件已经入库，未修改，即版本库中的文件快照内容与工作区中完全一致。 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果移出版本库, 则成为Untracked文件
- Modified: 已修改,。已经版本控制的文件在工作区中被修改了，还未加入暂存区。 这个文件也有两个去处, 通过git add可进入暂存staged状态。使用git checkout – 命令丢弃修改, 返回到unmodify状态, git checkout –命令是用暂存区的文件覆盖工作区文件
- Staged: 已暂存

## 举个🌰
下面就以`demo.txt`文件演示四个状态之间的流转。(爱动手的朋友可以每次执行完相应的命令再执行`git status`命令以验证)
```bash
# 新建文件。此时该文件状态为Untracked。
touch demo.txt
# 将其加入暂存区。此时该文件状态为Staged。
git add demo.txt
# 修改文件。此时该文件状态为Modified。
vim demo.txt
# 将修改后的文件加入暂存区。此时该文件状态为Staged。
git add demo.txt
# 提交文件后。此时该文件状态为Unmodified。
git commit -m "提交demo.txt"
# 取消跟踪demo.txt文件。此时该文件状态为Untracked。
git rm --cached demo.txt
```