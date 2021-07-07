+++
title = "Ubuntu20.04蓝牙没作用"
date = "2021-03-02"
author = "xblzbjs"
tags = [
  "Ubuntu",
  "Linux",
]
+++

## 前言

我的笔记本电脑是联想小新Air14-2020-AMD版，从Windows转到Ubuntu20.04时发现蓝牙扫描不到周围的设备（特别是耳机）！

执行了以下命令，蓝牙又重新工作了O(∩_∩)O~~

```shell
sudo rmmod btusb
sudo modprobe btusb
```

## 参考链接

[Ubuntu 20.04 bluetooth not working](https://askubuntu.com/questions/1231074/ubuntu-20-04-bluetooth-not-working)
