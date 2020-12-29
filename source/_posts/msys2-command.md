---
title: MSYS2 命令笔记
tags:
  - MSYS2
  - 命令
category: 计算机
date: 2020-12-29 12:57:02
index_img:
banner_img:
---


首先说说什么是 `MSYS2` ？

`MSYS2 （Minimal SYStem 2)` 是一个`MSYS`的独立改写版本，主要用于 `shell` 命令行开发环境。同时它也是一个在`Cygwin` （`POSIX` 兼容性层） 和 `MinGW-w64`（从"`MinGW`-生成"）基础上产生的，追求更好的互操作性的 `Windows` 软件。

## 常用命令

```shell
pacman -Sy 更新软件包数据
pacman -R package-name 删除软件包
pacman -S package-name 安装软件包
pacman -Syu 更新所有
pacman -Ss xx 查询软件xx的信息
```

## 安装软件

安装软件通过 ```pacman -S package-name``` 来安装。

如果不知道具体的软件名或要安装的版本信息，则可以先通过 ```pacman -Ss xx 查询软件xx的信息```命令，来先查一下软件的具体信息；因为有些软件包含很多的版本,所以通过该命令来找到具体的软件包名称后，通过复制软件名再通过 ```pacman -S``` 来安装。

## 官网

[https://www.msys2.org/](https://www.msys2.org/)
