+++
title = "Nmap 网络扫描工具常用操作"
date = "2021-03-17"
author = "xblzbjs"
tags = [
  "Linux",
  "Network",
]
+++

Nmap 是由 Gordon Lyon 创建的免费开源网络扫描工具。

## 安装

Debian系列的Linux，例如Ubuntu

```bash
sudo apt-get install nmap
```

Red Hat系列的Linux，例如Centos

```bash
sudo yum install nmap
```

## 常用功能

### 主机扫描

#### 单个主机

```bash
nmap baidu.com
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-16 13:56 CST
Nmap scan report for baidu.com (220.181.38.148)
Host is up (0.046s latency).
Other addresses for baidu.com (not scanned): 39.156.69.79
Not shown: 995 filtered ports
PORT    STATE SERVICE
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
143/tcp open  imap
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 5.99 seconds
```

上图可以看到扫描`baidu.com`的地址、开放端口、用了什么协议、请求时间等信息。

#### 多个主机

```bash
nmap baidu.com google.com
```

### 地址扫描

#### 单地址

```bash
nmap 主机地址
```

#### 多地址

```bash
nmap 主机地址1 主机地址2
```

#### 区间地址

```bash
nmap 192.168.18.68-100
```

### 文件扫描指定主机

假设有一个文件`hostlist.txt`，其内容如下

```
google.com
baidu.com
sougou.com
```

```bash
nmap -iL hostlist.txt
```

### 排除指定的主机

如果要扫描整个网络，但是要排除某些机器，可以使用 `--exclude` 參數：

```bash
nmap 192.168.0.* --exclude 192.168.0.100
```

若以文件方式指定主机，也可以使用 `--excludefile` 指定排除的列表：

```bash
nmap -iL hostlist.txt --excludefile excludelist.txt
```

### 测试主机是否有防火墙

Nmap 可以通过 TCP ACK 扫描，检测主机是否有启用防火牆：

```bash
nmap -PN scanme.nmap.org
```

### 快速扫描

快

```bash
nmap -F baidu.comStarting Nmap 7.80 ( https://nmap.org ) at 2021-06-16 14:40 CSTNmap scan report for baidu.com (220.181.38.148)Host is up (0.045s latency).Other addresses for baidu.com (not scanned): 39.156.69.79Not shown: 95 filtered portsPORT    STATE SERVICE25/tcp  open  smtp80/tcp  open  http110/tcp open  pop3143/tcp open  imap443/tcp open  httpsNmap done: 1 IP address (1 host up) scanned in 2.27 seconds
```

慢

```bash
nmap baidu.comStarting Nmap 7.80 ( https://nmap.org ) at 2021-06-16 14:40 CSTStats: 0:00:00 elapsed; 0 hosts completed (0 up), 1 undergoing Ping ScanPing Scan Timing: About 100.00% done; ETC: 14:40 (0:00:00 remaining)Nmap scan report for baidu.com (39.156.69.79)Host is up (0.074s latency).Other addresses for baidu.com (not scanned): 220.181.38.148Not shown: 995 filtered portsPORT    STATE SERVICE25/tcp  open  smtp80/tcp  open  http110/tcp open  pop3143/tcp open  imap443/tcp open  httpsNmap done: 1 IP address (1 host up) scanned in 7.75 seconds
```

### 扫描指定的端口

```bash
# 扫描80端口nmap -p 80 192.168.1.1# 指定端口范围nmap -p 80-200 192.168.1.1# 指定TCP的80端口nmap -p T:80 192.168.1.1# 指定UDP的53端口nmap -p U:53 192.168.1.1# 指定UDP和TCP的端口范围nmap -p U:53,111,137,T:21-25,80,139,8080 192.168.1.1# 扫描前10个常用的端口nmap --top-ports 10 192.169.1.1
```

### 查询主机名称

```bash
# 只查询nmap -sL 220.181.38.0/24
```

结果如下：

```bash
Nmap scan report for 220.181.38.0....Nmap scan report for mx10.baidu.com (220.181.38.155)...Nmap scan report for 220.181.38.247Nmap scan report for mx9.baidu.com (220.181.38.248)...Nmap done: 256 IP addresses (0 hosts up) scanned in 5.91 seconds
```

## 常用参数及其说明

| 参 数       | 说 明                                                        |
| ----------- | ------------------------------------------------------------ |
| -sT         | TCP connect()扫描，这种方式会在目标主机的日志中记录大批连接请求和错误信息。 |
| -sS         | 半开扫描，很少有系统能把它记入系统日志。不过，需要Root权限。 |
| -sF  -sN    | 秘密FIN数据包扫描、Xmas Tree、Null扫描模式                   |
| -sP         | ping扫描，Nmap在扫描端口时，默认都会使用ping扫描，只有主机存活，Nmap才会继续扫描。 |
| -sU         | UDP扫描，但UDP扫描是不可靠的                                 |
| -sA         | 这项高级的扫描方法通常用来穿过防火墙的规则集                 |
| -sV         | 探测端口服务版本                                             |
| -Pn         | 扫描之前不需要用ping命令，有些防火墙禁止ping命令。可以使用此选项进行扫描 |
| -v          | 显示扫描过程，推荐使用                                       |
| -h          | 帮助选项，是最清楚的帮助文档                                 |
| -p          | 指定端口，如“1-65535、1433、135、22、80”等                   |
| -O          | 启用远程操作系统检测，存在误报                               |
| -A          | 全面系统检测、启用脚本检测、扫描等                           |
| -oN/-oX/-oG | 将报告写入文件，分别是正常、XML、grepable 三种格式           |
| -T4         | 针对TCP端口禁止动态扫描延迟超过10ms                          |
| -iL         | 读取主机列表，例如，“-iL C:\ip.txt”                          |

