---
author:
  name: "xblzbjs"
title: "Vim｜搜索替换"
date: 2021-03-31
linktitle: vim｜search and replace
draft: false
type:
- article
- articles
series:
- Vim
---

## 单个文件搜索替换操作
`:[range]s[ubsitute]/{pattern}/{string}/[flags]`

### 参数：
- `range`: 表示范围。比如`:10,20`代表10-20行， `%`代表全部。
- `pattern`: 替换的模式.
- `string`: 替换后的文本。
- `flags`: 替换的标志。g表示全局范围内执行；c表示确认，可以确认或者拒绝修改；n报告匹配到的次数而不替换，可以用来查询匹配次数

### 举个🌰
```vim
# 把文件中world全部替换成WORLD
:% s/world/WORLD/g
# 统计1-20行 world单词出现的个数
:1,5 s/world//n
# 正则表达式精确匹配
:% s/\<world\>/WORLD/
```