---
author:
  name: "xblzbjs"
title: "Go｜testing.T"
date: 2021-03-24
linktitle: Go-testing.T
draft: false
type:
- article
- articles
categories: ["Golang"]
series:
- Go单元测试
---
## 因何而生❓

有时测试程序需要在测试之前或之后进行额外的设置或拆卸。有时测试还需要控制哪些代码在主线程上运行。为了支持这些和其他情况, `testing` 包提供了 `TestMain` 函数 :

```go
func TestMain(m *testing.M)
```

## 有什么作用🤔

如果测试文件中包含该函数，那么生成的测试将调用 `TestMain(m)`，而不是直接运行测试。`TestMain` 运行在主 goroutine 中 , 可以在调用 `m.Run` 前后做任何设置和拆卸。`m. Run`将返回可能传递给的退出代码操作系统退出. 如果TestMain返回，测试包装器将m.Run的结果传递给操作系统退出它自己

调用TestMain时，flag.解析尚未运行。如果TestMain依赖于命令行标志，包括测试包的标志，那么它应该调用flag.解析明确地。命令行标志总是由运行的时间测试或基准函数来解析

因此，当您需要为测试执行一些全局设置/删除时，这可能会很方便

## 举个🌰

### 简单

```go
// file name: demo_test.go
package tests

import (
    "testing"
    "os"
)

func TestMain(m *testing.M) {
    log.Println("Do stuff BEFORE the tests!")
    exitVal := m.Run()
    log.Println("Do stuff AFTER the tests!")
    os.Exit(exitVal)
}

func TestA(t *testing.T) {
    log.Println("TestA running")
}

func TestB(t *testing.T) {
    log.Println("TestB running")
}
```

#### 输出

```bash
$ go test -v demo_test.go
2021/03/24 11:01:06 Do stuff BEFORE the tests!
=== RUN   TestA
2021/03/24 11:01:06 TestA running
--- PASS: TestA (0.00s)
=== RUN   TestB
2021/03/24 11:01:06 TestB running
--- PASS: TestB (0.00s)
PASS
2021/03/24 11:01:06 Do stuff AFTER the tests!
```

### 复杂

假设在测试之前需要对数据库进行一些配置，不使用`Testmain`函数可能需要这么写

```go
func TestDBFeatureA(t *testing.T) {
    models.TestDBManager.Setup()
    defer models.TestDBManager.Exit()

    // Do the tests
}
func TestDBFeatureB(t *testing.T) {
    models.TestDBManager.Setup()
    defer models.TestDBManager.Exit()

    // Do the tests
}
```

有了`Testmain`后，配置更为简单清晰

```go
func TestDBFeatureA(t *testing.T) {
    defer models.TestDBManager.Reset()

    // Do the tests
}
func TestDBFeatureB(t *testing.T) {
    defer models.TestDBManager.Reset()

    // Do the tests
}
func TestMain(m *testing.M) {
    models.TestDBManager.Setup()
    // os.Exit() does not respect defer statements
    code := m.Run()
    models.TestDBManager.Exit()
    os.Exit(code)
}
```

虽然每个测试都必须在自身完成后进行清理，但这只涉及恢复初始数据，这比进行模式迁移要快得多。这种方法还减少了代码重复，在每个测试中只有一行用于数据库管理，而不是两行，运行速度会快得多。

## 注意⚠️

- `testing.M`有一个名为`Run（）`的已定义函数，它运行包中的所有测试。`Run（）`返回可以传递给的退出代码操作系统退出.
- 在 `TestMain` 函数的最后，应该使用 `m.Run` 的返回值作为参数去调用 `os.Exit`(见官方例子)。
- 如果不调用`os.Exit`，那测试命令将返回0（测试失败）
- 由于函数名在包中需要唯一，因此只能为每个包定义一次`TestMain`。如果包下有多个测试文件，最好在`TestMain`函数中进行逻辑判断。

## 总结📒

`TestMain`函数很好，但并不是所有的包都需要实现`TestMain`。如果团队使用第三方测试框架仅仅用到了设置和拆卸功能，这种情况下就可以考虑简化，用go提供的`TestMain`函数来进行配置。