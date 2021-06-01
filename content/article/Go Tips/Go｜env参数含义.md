---
title: "Go｜env 参数含义"
date: 2021-03-24T11:13:12+08:00
draft: true
author: "[xblzbjs]"
categories: ["Golang"]
---


```bash
GO111MODULE="auto"
GOARCH="amd64"
GOBIN=""
GOCACHE="/Users/xblzbjs/Library/Caches/go-build"
GOENV="/Users/xblzbjs/Library/Application Support/go/env"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOINSECURE=""
GOMODCACHE="/Users/xblzbjs/go/pkg/mod"
GONOPROXY="*.applysquare.*"
GONOSUMDB="*.applysquare.*"
# 编译代码的操作系统名称。windows:windows,linux:linux,mac os:darwin
GOOS="darwin"
# 开发工作区(workspace),是存放源代码、测试文件、库静态文件、可执行文件的工作。(多个工作区以":"分割)
GOPATH="/Users/xblzbjs/go:/Users/xblzbjs/Documents/GitHub/"
GOPRIVATE="*.applysquare.*"
# go get 走的代理
GOPROXY="https://goproxy.cn,direct"
# go目录绝对路径。
GOROOT="/usr/local/go"
# go工具目录绝对路径
GOTOOLDIR=""
GOSUMDB="sum.golang.org"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/darwin_amd64"
GCCGO="gccgo"
AR="ar"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
GOMOD=""
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/36/mvpln6lx6bx1nxqhf_h3qkbh0000gn/T/go-build826751021=/tmp/go-build -gno-record-gcc-switches -fno-common"
```



| 参数 | 含义 |
| ---- | ---- |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |



##### GOOS和GOARCH的取值范围

GOOS和GOARCH的值成对出现，而且只能是下面列表对应的值。

```
$GOOS	    $GOARCH
android	    arm
darwin	    386
darwin	    amd64
darwin	    arm
darwin	    arm64
dragonfly   amd64
freebsd	    386
freebsd	    amd64
freebsd	    arm
linux	    386
linux	    amd64
linux	    arm
linux	    arm64
linux	    ppc64
linux	    ppc64le
linux	    mips
linux	    mipsle
linux	    mips64
linux	    mips64le
linux	    s390x
netbsd	    386
netbsd	    amd64
netbsd	    arm
openbsd	    386
openbsd	    amd64
openbsd	    arm
plan9	    386
plan9	    amd64
solaris	    amd64
windows	    386
windows	    amd64
```



[环境变量｜官方文档](https://golang.org/cmd/go/)

