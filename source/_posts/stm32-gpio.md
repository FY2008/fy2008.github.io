---
title: STM32 GPIO 寄存器
date: 2020-12-30 08:43:32
tags:
- STM32
- GPIO
- IO
- 寄存器
category: 电子技术
index_img: /img/stm32-gpio-1.png
banner_img: /img/stm32-gpio-1.png
---

本篇文章来了解下 `stm32` 的 `gpio` 寄存器，`STM32`的`GPIO`共有11个寄存器，下面来详细介绍：

每个通用 `I/O` 端口都有一下寄存器

* 4 个 32 位配置寄存器（`GPIOx_MODER`、`GPIOx_OTYPER`、`GPIOx_OSPEEDR` 和 `GPIOx_PUPDR`）
* 2个32位数据寄存器(`GPIOx_IDR` 和 `GPIOx_ODR`)
* 1个32位置位/复位寄存器(`GPIOx_BSRR`)
* 1个16位复位寄存器(`GPIOx_BRR`)
* 1个32位锁定寄存器(`GPIOx_LCKR`)
* 2 个 32 位复用功能选择寄存器（`GPIOx_AFRH` 和 `GPIOx_AFRL`）

## GPIO 主要特性

* 受控 `I/O` 多达 16 个
* 输出状态：推挽或开漏 + 上拉/下拉
* 从输出数据寄存器 (`GPIOx_ODR`) 或外设（复用功能输出）输出数据
* 可为每个 `I/O` 选择不同的速度
* 输入状态：浮空、上拉/下拉、模拟
* 将数据输入到输入数据寄存器 (`GPIOx_IDR`) 或外设（复用功能输入）
* 置位和复位寄存器 (`GPIOx_BSRR`)，对 `GPIOx_ODR` 具有按位写权限
* 锁定机制 (`GPIOx_LCKR`)，可冻结 `I/O` 配置
* 模拟功能
* 复用功能输入/输出选择寄存器（一个 `I/O` 最多可具有 16 个复用功能）
* 快速翻转，每次翻转最快只需要两个时钟周期
* 引脚复用非常灵活，允许将 `I/O` 引脚用作 `GPIO` 或多种外设功能中的一种

每个`I/O`端口位可以自由编程，然而必须按照32位字访问`I/O`端口寄存器(不允许半字或字节访问)。

`GPIOx_BSRR`和`GPIOx_BRR`寄存器允许对任何`GPIO`寄存器进行读/更改的独立访问；这样，在读和更改访问之间产生`IRQ`时不会发生危险。

## GPIO 功能描述
根据数据手册中列出的每个 I/O 端口的特性，可通过软件将通用 I/O (GPIO) 端口的各个端口 位分别配置为多种模式：

* 输入浮空
* 输入上拉
* 输入下拉
* 模拟功能
* 具有上拉或下拉功能的开漏输出
* 具有上拉或下拉功能的推挽输出
* 具有上拉或下拉功能的复用功能推挽
* 具有上拉或下拉功能的复用功能开漏

下图给出了一个`I/O`端口位的基本结构。

![STM32 GPIO](/img/stm32-gpio-1.png)

## I/O 端口控制寄存器
每个 `GPIO` 有 4 个 32 位存储器映射的控制寄存器（`GPIOx_MODER`、`GPIOx_OTYPER`、 `GPIOx_OSPEEDR`、`GPIOx_PUPDR`），可配置多达 16 个 `I/O`。

* `GPIOx_MODER` 寄存器用于 选择 I/O 方向（输入、输出、AF、模拟）。
* `GPIOx_OTYPER` 和 `GPIOx_OSPEEDR` 寄存器分 别用于选择输出类型（推挽或开漏）和速度 (无论采用哪种 I/O 方向，都会直接将 I/O 速度引 脚连接到相应的`GPIOx_OSPEEDR` 寄存器位）。无论采用哪种 `I/O` 方向，`GPIOx_PUPDR` 寄 存器都用于选择上拉/下拉。
* `GPIOx_PUPDR` 寄存器都用于选择上拉/下拉

## I/O 端口数据寄存器
每个 `GPIO` 都具有 2 个 16 位数据寄存器：输入和输出数据寄存器（`GPIOx_IDR` 和 `GPIOx_ODR`）。`GPIOx_ODR` 用于存储待输出数据，可对其进行读/写访问。通过 `I/O` 输入的数据存储到输入数据寄存器 (`GPIOx_IDR`) 中，它是一个**只读寄存器**。

