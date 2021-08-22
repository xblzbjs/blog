+++
title = "Go & Python｜结构型设计模式"
date = "2021-03-20"
author = "xblzbjs"
categories = [
    "设计模式",
]
tags = [
  "Go",
  "Python",
]
+++


## 结构型模式

常见的结构型设计模式：

1. 工厂模式（Factory）：解决对象创建问题
2. 构造模式（Builder）：控制复杂对象的创建
3. 原型模式（Prototype）：通过原型的克隆创建新的实例
4. 单例（Borg/Singleton）：一个类只能创建同一个对象
5. 对象池模式（Pool）：预先分配同一类型的一组实例
6. 惰性计算模式（Lazy Evaluation）：延迟计算（python的property）

### 装饰器模式

什么是装饰器（Decorator）

- Python中一切皆对象，函数也可以当作参数传递
- 装饰器是接受函数作为参数，添加功能后返回一个新函数的函数（类）
- 通过@使用装饰器

```python
import time

def log_time(func): # 接受一个函数作为参数
    def _log(*args, **kwargs):
        beg = time.time()
        res = func(*args, **kwargs)
        print('use time:{}'.format(time.time()-beg))
        return res
    return _log

@log_time # @:装饰器语法糖
def mysleep():
    time.sleep(1)

newsleep = log_time(mysleep) # 等价于mysleep()
newsleep()
mysleep()
```

```python
import time

# 装饰器类实现
class LogTime:
    
    def __init__(self, use_int=False):
        self.use_int = use_int # 增加参数
    
    def __call__(self,func):
        def _log(*args, **kwargs):
            beg = time.time()
            res = func(*args, **kwargs)
            if self.use_int:
                print('use time:{}'.format(int(time.time()-beg))
            else:
                print('use time:{}'.format(time.time()-beg)
            return res
        return _log
    
@LogTime(True)
def mysleep():
    time.sleep(1)
    
mysleep()
```

### 2. 代理模式

什么是代理模式（Proxy）

- 把一个对象的操作代理到另一个对象
- 通常使用has-a组合关系
- 经常使用在裸接口，架一个代理的间接层更加安全

```python
class Stack(object): # 使用组合的例子
    def __init__(self):
        self._deque = deque()
    
    def push(self, value):
        return self._deque.append(value)

    def pop(self):
        return self._deque.pop()
 
    def empty(self):
        return len(self._deque) == 0
```

### 3. 适配器模式

什么是适配器模式（Adapter）

- 把不同对象的接口适配到同一个接口（多功能充电头）
- 当我们需要给不同的对象统一接口的时候可以使用适配器模式

```python
class Dog(object):
    def __init__(self):
        self.name = "Dog"

    def bark(self):
        return "woof!"

class Cat(object):
    def __init__(self):
        self.name = "Cat"

    def meow(self):
        return "meow!"

class Adapter:
    def __init__(self, obj, **adapted_methods):
        self.obj = obj
        self.__idct__.update(adapted_methods)

    def __getattr__(self, attr):
        return getattr(self.obj, attr)

objects = []
dog = Dog()
objects.append(Adapter(dog, make_noise=dog.bark))
cat = Cat()
objects.append(Adapter(cat, make_noise=cat.meow))
for obj in objects:
    # make_noise() 作为统一接口
    print("A {0} goes {1}".format(obj.name, obj.make_noise()))
```
