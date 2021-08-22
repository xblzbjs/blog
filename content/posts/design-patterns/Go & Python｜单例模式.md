+++
title = "Go & Python｜单例模式"
date = "2021-03-21"
author = "xblzbjs"
categories = [
    "设计模式",
]
tags = [
  "Go",
  "Python",
]
+++


## 简介

单例模式（Singleton Pattern）是一种创建型设计模式。单例模式保证了在程序的不同位置都**可以且仅可以取到同一个对象实例**：如果实例不存在，会创建一个实例；如果已存在就会返回这个实例。

### 优点

- 减少内存与性能开销
- 防止多个实例产生冲突

### 缺点

- 在代码中隐藏了程序的依赖项，而不是通过接口公开它们。
- 违反了单一职责原则
- 该模式在多线程环境下需要进行特殊处理， 避免多个线程多次创建单例对象。
- 单例的客户端代码单元测试可能会比较困难， 因为许多测试框架以基于继承的方式创建模拟对象。 由于单例类的构造函数是私有的， 而且绝大部分语言无法重写静态方法， 所以你需要想出仔细考虑模拟单例的方法。

### 适合场景

因为单例模式保证了实例的全局唯一性，而且只被初始化一次，所以比较适合**全局共享一个实例，且只需要被初始化一次**的场景。例如

- 数据库实例配置
- 全局配置
- 全局任务池

### 饿汉方式与懒汉方式

单例模式又分为**饿汉方式**和**懒汉方式**。饿汉方式指全局的单例实例在包被加载时创建，而懒汉方式指全局的单例实例在第一次被使用时创建。接下来，我就来分别介绍下这两种方式。

#### 饿汉方式

饿汉方式指全局的单例实例在包被加载时创建。需要注意，采用饿汉方式初始化耗时，会导致程序加载时间比较长。

#### 懒汉方式

懒汉方式指全局的单例实例在第一次被使用时创建。

工程项目中，懒汉方式使用的频率要更多一些。但它的缺点是非并发安全，在实际使用时需要加锁。

#### 饿汉方式 VS 懒汉方式

|          | 自动初始化 | 是否需要加锁 | 多线程安全 | 实现难度 |
| -------- | ---------- | ------------ | ---------- | -------- |
| 饿汉方式 | 是         | 否           | 是         | 易       |
| 懒汉方式 | 否         | 是           | 是         | 中       |

## Go最佳实践

### 饿汉方式

```go
package singleton

type singleton struct {
}

var ins *singleton = &singleton{}

func GetInsOr() *singleton {
	return ins
}

func init() {
	GetInsOr()
}
```

### 懒汉方式

```go
type Singleton struct {
	data string
}

var ins *Singleton
var once sync.Once

func GetSingletonObj() *Singleton {
	once.Do(func() {
		fmt.Println("Create a singleton object")
		singleInstance = new(Singleton)
	})
	return singleInstance
}
```

使用`once.Do`可以确保 `ins` 实例全局只被创建一次，`once.Do `函数还可以确保当同时有多个创建动作时，只有一个创建动作在被执行。

## Python最佳实践

值得一提的是，python的`import`模块设计上是天然的单例模式。下面使用最常用的`__new__`方法实现单例模式

### 饿汉方式

```python
class Singleton(object):
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls.instance = super().__new__(cls, *args, **kwargs)
        return cls._instance
 
class MyClass(Singleton):
    pass

c1 = MyClass()
c2 = MyClass()
assert c1 is c2 
```

### 懒汉方式

使用装饰器（decorator），更pythonic、优雅的方法

```python
def singleton(cls, *args, **kwargs):
    instances = {}

    def _singleton():
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]

    return _singleton

@singleton
class MyClass(object):
    a = 1

    def __init__(self, x=0):
        self.x = x


c1 = MyClass()
c2 = MyClass()
assert c1 is c2
```

