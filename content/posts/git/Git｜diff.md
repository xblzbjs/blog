+++
title = "Git｜diff"
date = "2021-03-23"
author = "xblzbjs"
tags = [
    "Git",
]
+++

## 假设你的仓库下有两个分支`main`和`dev`

场景一：

### 比较工作区

```bash
# 显示头和工作目录之间的差异。
git diff HEAD
# 
```

### 比较文件

```bash
# 比较工作区与最后一次commit提交的仓库的共同文件
git diff 
# 显示暂存区(已add但未commit文件)和最后一次commit(HEAD)之间的所有不相同文件的增删改
git diff --cached


```

### 比较分支

```bash
# 显示main和dev有差异的文件(显示更改代码)
git diff main dev
# 显示main和dev有差异的文件(不显示更改代码)
git diff main dev --stat
```
