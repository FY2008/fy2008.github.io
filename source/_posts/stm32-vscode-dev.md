---
title: 用 VSCode 开发 STM32 项目
tags:
  - STM32
  - vscode
category: 电子技术
date: 2020-12-29 00:38:30
index_img: /img/stm32.png
banner_img: /img/vscode.png
---


vscode 是一款最好用的开发工具之一，该软件非常好用强大，通过安装插件几乎可以完成所有的开发工作。

用 vscode 开发 stm32 的步骤是：

1. 用`STM32CubeMX` 生成 `stm32` 项目，注意要生成 `Makefile` 项目
2. 使用 stm32-for-vscode 插件来 {% label primary@编译 %} {% label primary@调试 %} {% label primary@下载 %} 代码。
3. 添加自己的 `c` 源文件和头文件，并把这些文件的路径添加到 `Makefile` 中

所以要维护的也就是 `Makefile` 文件。

## 添加 -u_printf_float -u_sprintf_float

添加串口打印浮点数的功能

在 Makefile 中这行代码前添加

```makefile
LDFLAGS = $(MCU) -u_printf_float -u_sprintf_float -specs=nano.specs -T$(LDSCRIPT) $(LIBDIR) $(LIBS) -Wl,-Map=$(BUILD_DIR)/$(TARGET).map,--cref -Wl,--gc-sections
```
