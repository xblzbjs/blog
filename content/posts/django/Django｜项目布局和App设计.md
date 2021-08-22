+++
title = "Django｜项目布局和App设计"
date = "2021-03-20"
author = "xblzbjs"
tags = [
    "Django",
    "Python",
]
+++



有许多项目模板可以启动Django项目。

- [cookiecutter-django](https://github.com/pydanny/cookiecutter-django)

- [Django包](https://djangopackages.org/)

## 项目布局

### 默认的项目布局和更好的项目布局

```shell
# 默认项目布局 
mysite/
├── manage.py
├── my_app
│ ├── __init__.py
│ ├── admin.py
│ ├── apps.py
│ ├── migrations
│ │ └── __init__.py
│ ├── models.py
│ ├── tests.py
│ └── views.py
└── mysite
├── __init__.py
├── asgi.py
├── settings.py
├── urls.py
└── wsgi.py

# 更好的项目布局
<repository_root>/
├── <configuration_root>/
├── <django_project_root>/
```

#### 仓库根目录 repository_root

*<repository_root>*目录是项目的绝对根目录. 除了*<django_project_root>* 和 *<configuration_root>*, 我们还包括其他像*README.md*, *docs/* directory, *manage.py*, *.gitignore*, *requirements.txt* 文件和其他高级文件部署和运行项目所需的。

> 一些开发人员喜欢将<django_project_root>合并到项目的<repository_root>中。

#### Django项目根目录 django_project_root

如果使用 `django-admin.py startproject`，它生成的Django项目将成为项目根目录。

```shell
mysite/
├── manage.py
├── my_app
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

#### 配置根目录 configuration_root

<configuration_root>目录是放置设置模块和基本URLConf (urls.py)的地方。这必须是一个有效的Python包(包含`__init__.py`)

### 更好的项目布局示例

```shell
mysite_project
├── config/
│   ├── settings/
|   |   ├── __init__.py
|   |   ├── base.py
|   |   ├── local.py
|   |   ├── staging.py
|   |   ├── test.py
|   |   ├── production.py
│   ├── __init__.py
|   ├── asgi.py
│   ├── urls.py
│   └── wsgi.py
├── docs/
├── mysite/
│   ├── media/ # Development only!
│   ├── products/
│   ├── profiles/
│   ├── ratings/
│   ├── static/
│   └── templates/
├── .gitignore
├── Makefile
├── README.rst
├── manage.py
└── requirements/
|   ├── base.txt
|   ├── local.txt
|   ├── staging.txt
|   ├── production.txt
```

| File or Directory    | Purpose                                                      |
| -------------------- | ------------------------------------------------------------ |
| .gitignore           | 列出Git应该忽略的文件和目录。这个文件对于其他版本控制系统是不同的。例如，如果使用的是Mercurial，则会有一个.hgignore文件。) |
| config/              | 项目的<configuration_root>，其中包含项目范围的*sttings*、*url*和*wsgi.py*模块 |
| Makefile             | 包含简单的部署任务和宏。                                     |
| manage.py            | 如果您保留此选项，请不要修改其内容                           |
| README.rst and docs/ | 项目文档                                                     |
| requirements.txt     | 项目所需的Python包列表                                       |
| mysite/              | 项目的<django_project_root>                                  |

| File or Directory | Purpose                                                      |
| ----------------- | ------------------------------------------------------------ |
| media/            | 仅用于开发:用户生成的静态媒体资产，如用户上传的照片。对于较大的项目，它将托管在单独的静态媒体服务器上 |
| products/         | 管理和显示应用程序                                           |
| profiles/         | 管理和显示用户档案的应用程序。                               |
| ratings/          | 管理用户评级。                                               |
| static/           | 非用户生成的静态文件，包括CSS、JavaScript和图像。对于较大的项目，它将托管在单独的静态媒体服务器上 |
| templates/        | Django模板文件                                               |

#### Setting和Requirement文件

使用一个包含多种环境配置文件的settings/目录，而不是使用一个settings.py文件。

```python
settings/
├── __init__.py
├── base.py
├── local.py
├── staging.py
├── test.py
├── production.py
```

| Setting files | Purpose                                                      |
| ------------- | ------------------------------------------------------------ |
| base.py       | 项目通用的设置。                                             |
| local.py      | 本地处理项目时使用的设置文件。本地特定于开发的设置包括调试模式、日志级别和像django-debug-toolbar这样的开发人员工具 |
| staging.py    | 准备版本，用于在生产服务器上运行站点的半私有版本。这是经理和客户在将您的工作转移到生产环境之前应该注意的地方。 |
| test.py       | 运行测试的设置，包括测试运行程序、内存中的数据库定义和日志设置 |
| production.py | 这是生产服务器使用的设置文件。此文件仅包含生产级设置。有时被写为prod.py |

大型项目中工作，不同的开发人员需要不同的设置，而与队友共享相同的local.py设置模块是不行的。一个很好的方法是使用多个设置文件，例如local_team1.py和local_team2.py

```python
# 多个设置文件
settings/
    __init__.py
    base.py
    local_team1.py
    local_team2.py
    local.py
    staging.py
    test.py
    production.py
```

首先在<repository_root>中创建一个requirements/目录。然后创建”。与设置目录内容匹配的txt文件。结果应该是这样的:

```python
requirements/
├── base.txt
├── local.txt
├── staging.txt
├── production.txt
```


除了密码和API密钥之外的所有内容都应该在版本控制中。任何面向实际生产服务器的项目都需要多个设置和需求文件。即使是初学者，也需要进行这种设置/需求文件设置，这是一种好习惯。

### 扩展

```shell
# Django3 Web开发指南书中的例子
myproject_website
├── commands/
├── db_backups/ #数据库
├── mockups/
├── src/
│   └── django-myproject/
│       ├── externals/ # 使用pip安装项目所包含外部依赖的externals目录
│       │   ├── apps/   
│       │   │   └── README.md 
│       │   └── libs/
│       │       └── README.md
│       ├── locale/  # 项目翻译
│       ├── media/  # 媒体文件
│       ├── myproject/
│       │   ├── apps/
│       │   │   ├── core/ # 项目内建的Django应用，项目共享功能推荐名为core或utils的应用
│       │   │   │   ├── __init__.py
│       │   │   │   └── versioning.py
│       │   │   └── __init__.py
│       │   ├── settings/ #项目设置
│       │   │   ├── __init__.py
│       │   │   ├── _base.py   # 基本设置
│       │   │   ├── dev.py    # 开发
│       │   │   ├── production.py  # 生产
│       │   │   ├── sample_secrets.json # 密钥、数据库密码等敏感信息
│       │   │   ├── secrets.json
│       │   │   ├── staging.py   # 预发布
│       │   │   └── test.py    # 测试
│       │   ├── site_static/ # 项目特有静态文件
│       │   │   └── site/
│       │   │       ├── css/ # CSS 
│       │   │       │   └── style.css
│       │   │       ├── img/ # 样式图片、favicon和logo
│       │   │       │   ├── favicon-16x16.png
│       │   │       │   ├── favicon-32x32.png
│       │   │       │   └── favicon.ico
│       │   │       ├── js/  # js文件
│       │   │       │   └── main.js
│       │   │       └── scss/ # Sass文件
│       │   │           └── style.scss
│       │   │       └── vendor/ # 组合各种类型文件的第三方模块(如TinyMCE富文本编辑器)
│       │   │           
│       │   ├── templates/  # 项目HTML模板
│       │   │   ├── base.html # 继承模板
│       │   │   └── index.html # 主页
│       │   ├── __init__.py
│       │   ├── urls.py   # 项目URL配置
│       │   └── wsgi.py   # 项目web服务配
│       ├── requirements/  
│       │   ├── _base.txt  # 共享
│       │   ├── dev.txt   # 开发 
│       │   ├── production.txt # 生产
│       │   ├── staging.txt  # 预发布
│       │   └── test.txt  # 测试
│       ├── static/  # 存放静态文件
│       ├── LICENSE  # 开源许可证
│       └── manage.py*
└── venv/
```

## App 设计

> “The art of creating and maintaining a good Django app is that it should follow the
>
> truncated Unix philosophy according to Douglas McIlroy: ‘Write programs that do one
>
> thing and do it well.”

每个应用程序都应该专注于自己的任务

## App命名

> 不要太担心让应用程序设计完美。 这是一门艺术，而不是一门科学。 有时候你得重写它们或者把它们拆散。 没关系的。

如果您希望站点的博客出现在http://www.example.com/weblog/，那么考虑应用程序命名为weblog，而不是blog、posts或blogposts。更容易看到哪个应用程序对应于站点的哪个部分。

使用有效的、符合PEP8的、可导入的Python包名：短的、所有小写的名称，没有数字、破折号、句点、空格或特殊字符。 如果需要可读性，可以使用下划线来分隔单词，尽管不鼓励使用下划线

这是99%的Django应用程序中常见的模块。以斜杠（‘/’）结尾的任何模块都表示Python包，它可以包含一个或多个模块。

### Common modules

```shell
scoops/
├── __init__.py
├── admin.py
├── forms.py
├── management/
├── migrations/
├── models.py
├── templatetags/
├── tests/
├── urls.py
├── views.py
```

### Uncommon modules

```shell

scoops/
├── api/
├── behaviors.py
├── constants.py
├── context_processors.py
├── decorators.py
├── db/
├── exceptions.py
├── fields.py
├── factories.py
├── helpers.py
├── managers.py
├── middleware.py
├── signals.py
├── utils.py
├── viewmixins.py
```

| File or Directory | Purpose                                                      |
| ----------------- | ------------------------------------------------------------ |
| api/            | 该app的api模块 |
| behaviors.py         | 该app的model mixin |
| constants.py | 该app下的全局变量 |
| context_processors.py  | 上下文处理器    |
| decorators.py          | 该app的装饰器 |
| db/           | 自定义模型字段或组件的包 |
| exceptions.py        | Django模板文件                                               |
| fields.py | 通常用于该app的表单字段 |
| factories.py | 放置测试数据工厂的地方 |
| helpers.py | view层辅助函数 |
| managers.py | 当models.py变得太大时，一个常见的补救方法是将所有自定义模型管理器移到这个模块中。|
| middleware.py | 中间件 |
| schema.py | GraphQL api code |
| signals.py | 自定义信号 |
| utils.py | 该app的通用辅助函数 |
| viewmixins.py | view层可以通过将任何view mixin移动到这个模块来进行精简。|

### core app


 有时，我们会编写共享类或共享实用程序，这些共享类在任何地方都是通用的，不属于任何特定的app。 

我们处理这些实用程序的方法是将它们放到一个`core`的app中，`core`app主要包含用于跨项目使用的函数和对象，例子如下

```shell
core/
├── __init__.py
├── managers.py # 包含自定义model管理器
├── models.py
├── views.py # 包含自定义view mixin(s)
```

```python
# Importing From the Core App

from core.managers import <custom_manager>
from core.views import <custom_mixin>
```

将逻辑从更复杂的构造移动到放置在孤立模块中的函数的作用是：它变得更容易测试。 隔离意味着它通常是在应用程序中导入的，而不是在应用程序中/项目中导入。 这会减少业务逻辑开销，从而更容易为模块中的逻辑编写测试。

### Service layer

业务逻辑代码和相关服务也可以通过以下方式设计

``` shell
my_app/
├── api/
├── models.py
├── services.py # Service layer location for business logic
├── selectors.py # Service layer location for queries
├── tests/
```

## 