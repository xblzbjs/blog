+++
title = "Django｜编码规范"
date = "2021-03-20"
author = "xblzbjs"
tags = [
    "Django",
    "Python",
]
+++

## 编码风格 Coding Style

### 代码可读性

6个忠告:

➤ 避免缩写变量名

➤ 写函数参数名。

➤ 解释类和方法。

➤ 对代码进行注释

➤ 将重复的代码行重构为可重用的功能或方法

➤ 保持函数和方法简短。不应该滚动阅读整个函数或方法

### PEP8(**Python Enhancement Proposals**)

[PEP8文档](https://www.python.org/dev/peps/pep-0008/)

[PEP8中文翻译文档](https://blog.csdn.net/ratsniper/article/details/78954852)

### Python import 格式(以Django为例)

``` python
# future
from __future__ import unicode_literals

# 标准包
from math import sqrt
from os.path import abspath

# Django
from django.db import models
from django.utils.translation import ugettext_lazy as _

# 第三方包
from django_extensions.db.models import TimeStampedModel

# 自己写的包
from .models import BananaSplit


# try/except
try:
    import yaml
except ImportError:
    yaml = None

CONSTANT = 'foo'

class Example:
    # ...
```

> 在Django文档中‘Django’包和第三方包是调转的
>
> 在实际开发中不需要在导入包时进行注释

[django官方文档编码风格](https://docs.djangoproject.com/en/3.0/internals/contributing/writing-code/coding-style/)

另外，虽然官方标准中没有指定以下内容，但它们在Django社区中非常常见。

https://docs.djangoproject.com/en/3.0/internals/

### 使用显式的相对导入

不要使用硬编码方式导入包，例如下面这种：

```python
# myapp/views.py
from django.views.generic import CreateView

# 不好的例子
from cones.models import WaffleCone
from cones.forms import WaffleConeForm
from core.views import FoodMixin

class WaffleConeCreateView(FoodMixin, CreateView):
    model = WaffleCone
    form_class = WaffleConeForm
```

原因是硬编码方式会让代码的可重用性和可移植性变差

将包含隐式相对导入的坏代码片段转换为包含显式相对导入的好代码片段。下面是更正后的例子:

```python
# cones/views.py
from django.views.generic import CreateView

# 好的例子
from .models import WaffleCone
from .forms import WaffleConeForm
from core.views import FoodMixin

class WaffleConeCreateView(FoodMixin, CreateView):
    model = WaffleCone
    form_class = WaffleConeForm
```

### 避免使用import *与包冲突

```python
# 不好的例子
from django.forms import *
from django.db.models import *

# 好的例子
from django import forms
from django.db import models

```

原因是为了避免隐式地将另一个Python模块的所有局部变量加载到当前模块的名称空间中

包冲突

```python
# 不好的例子
from django.db.models import CharField
from django.forms import CharField

# 好的例子
from django.db.models import CharField as ModelCharField
from django.forms import CharField as FormCharField
```

### JS,HTML,CSS编码风格(扩展)

#### JavaScript 编码风格

不像Python有一个官方的风格指南，没有官方的JavaScript风格指南。相反，许多非官方的JS风格指南已经由不同的个人和/或公司创建:

➤ 普遍的JavaScript代码规范 https://github.com/feross/standard

➤ 标准的结合JavaScript和Node.js风格指南 https://github.com/feross/standard

➤ idiomatic.js: 编写一致的、惯用的JavaScript的原则 https://github.com/rwaldron/idiomatic.js/

➤ Airbnb JavaScript风格指南 https://github.com/airbnb/javascript

> ESLint ((<https://eslint.org/)是检查JavaScript和JSX代码样式的工具。它有几个样式指南的JS样式规则的预置，包括上面列出的一些。还有用于各种文本编辑器的ESLint插件和用于各种JavaScript工具的ESLint任务，如Webpack>, Gulp，和Grunt。

#### HTML 和 CSS 编码风格

http://codeguide.co

https://github.com/necolas/idiomatic-css

> Stylelint (<https://stylelint.io/)是CSS的编码样式格式化器。它检查与您配置它的规则的一致性，并检查您的CSS属性的排序顺序。就像ESLint一样，有stylelint文本编辑器和任务/>构建工具插件。
