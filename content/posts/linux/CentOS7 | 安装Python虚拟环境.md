+++
title = "CentOS7 | 安装Python虚拟环境"
date = "2021-03-17"
author = "xblzbjs"
tags = [
  "Linux",
  "CentOS",
]
+++

## virtualenv和virtualenvwrapper安装

### pip 3 安装

```shell
pip3 install virtualenv -i https://mirrors.aliyun.com/pypi/simple/
```

### 安装virtualenvwrapper工具管理虚拟环境

```shell
pip3 install virtualenvwrapper -i https://mirrors.aliyun.com/pypi/simple/

# 报错
[root@centos ~]# pip3 install virtualenvwrapper -i https://mirrors.aliyun.com/pypi/simple/
Looking in indexes: http://mirrors.tencentyun.com/pypi/simple
Collecting virtualenvwrapper
  Downloading http://mirrors.tencentyun.com/pypi/packages/c1/6b/2f05d73b2d2f2410b48b90d3783a0034c26afa534a4a95ad5f1178d61191/virtualenvwrapper-4.8.4.tar.gz (334 kB)
     |████████████████████████████████| 334 kB 832 kB/s 
    ERROR: Command errored out with exit status 1:
     command: /usr/local/python3/bin/python3.7 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-f3bmqs9b/virtualenvwrapper_891e91f27f1f44e4b94d4cb51de96c5d/setup.py'"'"'; __file__='"'"'/tmp/pip-install-f3bmqs9b/virtualenvwrapper_891e91f27f1f44e4b94d4cb51de96c5d/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /tmp/pip-pip-egg-info-u879bw4z
         cwd: /tmp/pip-install-f3bmqs9b/virtualenvwrapper_891e91f27f1f44e4b94d4cb51de96c5d/
    Complete output (11 lines):
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/usr/local/python3/lib/python3.7/site-packages/setuptools/__init__.py", line 19, in <module>
        from setuptools.dist import Distribution
      File "/usr/local/python3/lib/python3.7/site-packages/setuptools/dist.py", line 34, in <module>
        from setuptools import windows_support
      File "/usr/local/python3/lib/python3.7/site-packages/setuptools/windows_support.py", line 2, in <module>
        import ctypes
      File "/usr/local/python3/lib/python3.7/ctypes/__init__.py", line 7, in <module>
        from _ctypes import Union, Structure, Array
    ModuleNotFoundError: No module named '_ctypes'
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
```

原因：Python3中有个内置模块叫ctypes，它是Python3的外部函数库模块，它提供兼容C语言的数据类型，并通过它调用Linux系统下的共享库(Shared library)，此模块需要使用CentOS7系统中外部函数库(Foreign function library)的开发链接库(头文件和链接库)。由于在CentOS7系统中没有安装外部函数库(libffi)的开发链接库软件包，所以在安装pip的时候就报了"ModuleNotFoundError: No module named '_ctypes'"的错误。

```shell
# 解决方案
yum install libffi-devel -y

# 进入python3.7目录重新安装python
make&make install
```

### 创建虚拟环境(.virtualenvs)文件夹

```shell
mkdir .virtualenvs
```

### 配置bashrc

```shell
# 查找virtualenvwrapper.sh位置
[root@centos ~]# find / -name virtualenvwrapper.sh
/usr/local/python3/bin/virtualenvwrapper.sh

[root@centos ~]# vi ~/.bashrc
```

```shell
写入以下几行代码(export可以先去掉)

# 设置virtualenv的统一管理目录, 以后自动下载的虚拟环境，全部都放在这
export WORKON_HOME=/root/.virtualenvs 
# 添加virtualenvwrapper的参数，生成干净隔绝的环境(从版本20开始，默认就是'--no-site-packages')
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'   
# 指定python解释器的本体(注意此路径随不同的linux环境改变而改变)
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python   
# 执行virtualenvwrapper安装脚本
source /usr/local/python3/bin/virtualenvwrapper.sh 
```

```shell
加载配置文件

[root@centos ~]# source ~/.bashrc
virtualenvwrapper.user_scripts creating /root/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /root/.virtualenvs/postmkproject
virtualenvwrapper.user_scripts creating /root/.virtualenvs/initialize
virtualenvwrapper.user_scripts creating /root/.virtualenvs/premkvirtualenv
virtualenvwrapper.user_scripts creating /root/.virtualenvs/postmkvirtualenv
virtualenvwrapper.user_scripts creating /root/.virtualenvs/prermvirtualenv
virtualenvwrapper.user_scripts creating /root/.virtualenvs/postrmvirtualenv
virtualenvwrapper.user_scripts creating /root/.virtualenvs/predeactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/postdeactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/get_env_details
```

```shell
[root@centos ~]# mkvirtualenv test -p python3
created virtual environment CPython3.7.9.final.0-64 in 115ms
  creator CPython3Posix(dest=/root/.virtualenvs/test, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/root/.local/share/virtualenv)
    added seed packages: pip==20.3.1, setuptools==51.0.0, wheel==0.36.1
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
virtualenvwrapper.user_scripts creating /root/.virtualenvs/test/bin/predeactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/test/bin/postdeactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/test/bin/preactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/test/bin/postactivate
virtualenvwrapper.user_scripts creating /root/.virtualenvs/test/bin/get_env_details
(test) [root@VM-0-5-centos ~]# pip3 list
Package    Version
---------- -------
pip        20.3.1
setuptools 51.0.0
wheel      0.36.1
WARNING: You are using pip version 20.3.1; however, version 20.3.3 is available.
You should consider upgrading via the '/root/.virtualenvs/test/bin/python -m pip install --upgrade pip' command.

```
