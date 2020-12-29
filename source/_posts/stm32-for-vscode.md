---
title: 推荐一款 STM32 的 vscode 插件，stm32-for-vscode
date: 2020-12-29 14:29:35
tags:
- STM32
- vscode
- 插件
category: 电子技术
index_img: /img/stm32-for-vscode.png
banner_img: /img/10.jpg
---

[stm32-for-vscode](https://github.com/bmd-studio/stm32-for-vscode) 是一款用于使用 `vscode` 来开发 `stm32` 项目的插件，这款插件是 2019 首次发布的，属于比较新的插件了，通过测试可以使用。

## 先决条件
该插件可以实现快速的 {% label primary @编译 %} {% label primary @下载 %} {% label primary @调试 %} STM32 项目，先决条件是：

1. [Cortex-Debug](https://github.com/Marus/cortex-debug)，安装这个 vscode 扩展和基础的C/C++插件
2. [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)，安装该软件，并配置好环境变量
3. [Make](http://gnuwin32.sourceforge.net/packages/make.htm) ，电脑上要有 make.exe，并且配好环境变量
4. [OpenOCD](https://gnutoolchains.com/arm-eabi/openocd/)，安装 OpenOCD，并配置环境变量

以上4条是先决条件，接下来使用 `ST`官方提供的 `STM32CubeMX` 来创建一个项目，在项目配置 `Toolchain / IDE` 处选择 `Makefile`。

创建完成项目后，用`vscode`打开，执行 `ctrl+shift+p` 打开命令面板，在面板中输入 "stm32"后会出现三个命令，分别为：
![stm32 for vscode](/img/stm32-for-vscode.png)

第一次执行编译命令后，会在项目中生成 `.vscode` 文件夹，文件夹中包含一下几个文件:

```
c_cpp_properties.json
launch.json
settings.json
tasks.json
```

这几个文件就是 `stm32-for-vscode` 这几个插件生成的，默认也是配置好的，可以不用修改直接使用。

## 给任务添加快捷键

`ctrl+1` : 编译
`ctrl+2` : 下载
`ctrl+3` : Clean 与 编译

keybindings.json
```json
[
    {
        "key": "ctrl+1",
        "command": "workbench.action.tasks.runTask",
        "args": "Build STM",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+2",
        "command": "workbench.action.tasks.runTask",
        "args": "Flash STM",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+3",
        "command": "workbench.action.tasks.runTask",
        "args": "Build Clean STM",
        "when": "editorTextFocus"
    }
]
```

* **key** 表示绑定的键。
* **command** 表示执行的命令。
* **args** 命令的参数，这里我们是build编译任务
* **when** 快捷键在何时生效，这里指的是编辑区
