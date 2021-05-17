---
title: "创建型设计模式"
date: 2021-03-20T13:45:56+08:00
draft: false
author: "[xblzbjs]"
categories: ["Design-Patterns"]
tags: ["python", "golang"]
---

 

## 常见创建型设计模式

1. 工厂模式（Factory）：解决对象创建问题
2. 构造模式（Builder）：控制复杂对象的创建
3. 原型模式（Prototype）：通过原型的克隆创建新的实例
4. 单例（Borg/Singleton）：一个类只能创建同一个对象
5. 对象池模式（Pool）：预先分配同一类型的一组实例
6. 惰性计算模式（Lazy Evaluation）：延迟计算（python的property）

### 1.工厂模式

解决了什么问题：

- 解决对象创建问题
- 解耦对象的创建和使用
- 包括工厂方法和抽象工厂

```python
# 工厂方法
class DogToy:
    def speak(self):
        print("wang wang")
        
class CatToy:
    def speak(self):
        print("miao miao")
        
def toy_factory(toy_type):
    if toy_type == 'dog':
        return DogToy()
    elif toy_type == 'cat':
        return CatToy()
        
```

## 2.构造模式

什么是构造模式：

- 用来控制复杂对象的构造
- 创建和表示分离。比如你要买电脑，工厂模式直接给你需要的电脑
- 但是构造模式允许你自己定义电脑的配置，组装完成后给你

## 3.原型模式

什么是原型模式：

1. 通过克隆原型来创建新的实例
2. 可以通过相同的原型，通过修改部分属性来创建新的实例
3. 用途：对于一些创建实例开销比较高的地方可以用原型模式

## 4.单例模式（重点）

单例模式的实现由多种方式

- 单例模式：一个类创建出来的对象都是同一个
- Python的模块其实就是单例的，只会导入一次（import）
- 使用共享同一个实例的方式来创建单例模式

```python
# 单例模式
class Singleton:
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            _instance = super().__new__(cls, *args, **kwargs)
            cls.instance = _instance
            return cls._instance

class MyClass(Singleton):
    pass

c1 = MyClass()
c2 = MyClass()
assert c1 is c2 # c1, c2是同一个实例
```

## 参考

<https://design-patterns.readthedocs.io/zh_CN/latest/creational_patterns/creational.html>
