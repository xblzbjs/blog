---
title: "Markdown｜超链接、转义字符、表格"
date: 2020-03-19T13:36:28+08:00
draft: false
author: "[xblzbjs]"
categories: ["工具"]
tags: ["markdown"]
---

## 超链接
有文字链接、索引链接、图片链接([地址]({{<ref "/article/Markdown｜代码、代码块、列表、图片.md">}}))、自动链接
### Input
```
# 文字链接
[xblzbjs的博客](https://xblzbjs.cn "xblzbjs的博客")
# 索引链接
[xblzbjs的博客][1]
[1]:https://xblzbjs.cn
# 自动链接
<https://xblzbjs.cn>

```

### Output

- [xblzbjs的博客](https://xblzbjs.cn "xblzbjs的博客")
- [博客]: https://xblzbjs.cn "xblzbjs的博客"
[我的博客][博客]
[博客][]
- <https://xblzbjs.cn>


## 转义字符
### Input
```
\\ 反斜杠

\` 反引号

\* 星号

\_ 下划线

\{\} 大括号

\[\] 中括号

\(\) 小括号

\# 井号

\+ 加号

\- 减号

\. 英文句号

\! 感叹号
```
### Output
\\ 反斜杠

\` 反引号

\* 星号

\_ 下划线

\{\} 大括号

\[\] 中括号

\(\) 小括号

\# 井号

\+ 加号

\- 减号

\. 英文句号

\! 感叹号

## 表格

### Input
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
### Output
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
