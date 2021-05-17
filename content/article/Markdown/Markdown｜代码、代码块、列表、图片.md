---
title: "Markdown｜代码(代码块)、列表、图片"
date: 2020-03-19T13:36:28+08:00
draft: false
author: "[xblzbjs]"
categories: ["Markdown"]
tags: ["markdown"]
---


## 代码(代码块)
### Input:
```
`代码内容`
    ```(括号内可以设置代码块的格式，例如python,java,go,shell等)
    代码块
    ```
```
### Output:
`Hello, World!`
```python
Hello,World
```

## 列表
列表分为无序列表和有序列表，无序列表用使用“-”、“+”、“*”任何一种都可以，有序列表使用数字加点“1.”的形式

### 无序列表
### Input
```
- 列表内容
+ 列表内容
* 列表内容
```
### Output
- 列表内容
+ 列表内容
* 列表内容

### 有序列表
“.”跟内容之间要有空格
#### Input
```
1. 列表内容
2. 列表内容
3. 列表内容
```
#### Output
1. 列表内容
2. 列表内容
3. 列表内容
## 图片
### Input

```
![图片alt](图片链接 "图片title")
example
![无法显示图片](https://cdn.pixabay.com/photo/2021/02/08/16/03/dinosaur-5995333_960_720.png "cute")
```

### Output
![图片alt](https://cdn.pixabay.com/photo/2021/02/08/16/03/dinosaur-5995333_960_720.png "cute")
