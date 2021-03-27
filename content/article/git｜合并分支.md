---
title: "Git｜合并分支"
date: 2021-03-19T11:41:13+08:00
draft: false
author: "[xblzbjs]"
categories: ["版本控制"]
tags: ["git"]
---

首先查看当前项目有哪些分支`git branch -a`等同于 `git branch -all`

```bash
git branch -a 
```

输入后会看到当前项目的所有分支（其中带`*`的分支为当前分支）

```
dev
*main
```

假设你需要在`dev`分支上进行开发，则需要切换到该分支

```bash
git checkout dev
```

这个时候再使用`git branch -a`命令就会发现当前分支为`dev`分支

```git
*dev
main
```

在`dev`分支上进行开发，提交测试没有问题想合并到`main`分支上，需要进行以下几步:

1. 查看当前分支的状态并

   ```bash
   git status
   ```

2. 添加更改文件`push`到当前分支上

   ```bash
   git add . && git commit -m "update"
   ```

3. 将`dev`分支合并到`main`分支

   ```bash
   git merge dev
   ```

4. 检查main分支是否和dev分支的区别（当前处于`div`分支）

   ```bash
   git diff main
   ```

