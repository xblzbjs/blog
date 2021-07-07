+++
title = "Django｜设计"
date = "2021-03-20"
author = "xblzbjs"
tags = [
    "Django",
    "Python",
]
+++



有许多项目模板可以启动Django项目。这里有两个链接，可使用时，我们引导一个项目:

- [cookiecutter-django](https://github.com/pydanny/cookiecutter-django)

- [Django包](https://djangopackages.org/)

## 默认的项目布局和更好的项目布局

```python
# 默认项目布局 
<django_project_root>

# 更好的项目布局
<repository_root>/
├── <configuration_root>/
├── <django_project_root>/
```

### 仓库根目录 repository_root

*<repository_root>*目录是项目的绝对根目录. 除了*<django_project_root>* 和 *<configuration_root>*, 我们还包括其他关键组件像*README.md*, *docs/* directory, *manage.py*, *.gitignore*, *requirements.txt* 文件和其他高级文件部署和运行项目所需的。

> 一些开发人员喜欢将<django_project_root>合并到项目的<repository_root>中。

### Django项目根目录 django_project_root

如果使用 `django-admin.py startproject`，它生成的Django项目将成为项目根目录。

```python
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

### 配置根目录 configuration_root

<configuration_root>目录是放置设置模块和基本URLConf (urls.py)的地方。这必须是一个有效的Python包(包含`__init__.py`)

如果使用django-admin.py startproject，配置根最初是在Django项目根目录中。它应该移动到存储库根。

### 项目布局示例

```python
mysite_project
├── config/
│   ├── settings/
│   ├── __init__.py
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
└── requirements.txt
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

## 扩展

```python
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

```python 
# Common modules
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

```python
# uncommon modules
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

