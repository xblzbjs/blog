+++
title = "sed"
date = "2021-07-03"
author = "xblzbjs"
tags = [
  "Linux",
  "Shell",
]
+++

## 简介

sed（Stream Editor），流编辑器。对标准输出或文件逐行进行处理

## 语法

### 第一种形式

```shell
stdout | sed [option] "pattern command"
```

### 第二种形式

```shell
sed [option] "pattern command" file
```

## option选项

| 选项       | 含义                               |
| ---------- | ---------------------------------- |
| -n         | 只打印模式匹配行                   |
| -e         | 直接在命令行进行sed编辑，默认选线  |
| -f         | 编辑动作保存在文件中，指定文件执行 |
| -r         | 支持扩展正则表达式                 |
| -i（常用） | 直接修改文件内容                   |

## pattern匹配模式

| 匹配模式                             | 含义                                             |
| ------------------------------------ | ------------------------------------------------ |
| 10command                            | 匹配到第10行                                     |
| 10,20command                         | 匹配从第10行开始，到第20行结束 (10,20]           |
| 10,+5command                         | 匹配从第10行开始，到第16行结束 (10, 15]          |
| /pattern1/command（常用）            | 匹配到pattern1的行                               |
| /pattern1/,/pattern2/command（常用） | 匹配到pattern1的行开始，到匹配到pattern2的行结束 |
| 10, /pattern1/command                | 匹配从第10行开始，到匹配到pattern1的行结束       |
| /pattern1/,10command                 | 匹配到pattern1的行开始，到第10行匹配结束         |

## Command编辑命令

| 类别 | 命令 | 含义                   |
| ---- | ---- | ---------------------- |
| 查询 | p    | 打印                   |
| 增加 | a    | 行后增加               |
|      | i    | 行前追加               |
|      | r    | 外部文件读入，行后追加 |
|      | w    | 匹配行写入外部文件     |
| 删除 | d    |                        |

