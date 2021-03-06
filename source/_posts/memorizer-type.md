---
title: 存储器类型介绍
date: 2020-12-30 12:55:29
tags:
- 存储器
- ROM
- RAM
- FLASH
- SDRAM
category: 计算机
index_img: /img/nand-flash.jpg
banner_img:
---

在学习单片机的时候，老是忘记各种存储器`RAM`,`ROM`,`FLASH`等等类型的一些区别，这里做个笔记来记录一些常用的存储器类型。

存储器按是否易失分为：易失型和非易失型

## 易失型 RAM

* DRAM
  * SDRAM
  * DDR SDRAM
  * DDRII SDRAM
  * DDRIII SDRAM
* SRAM

### DRAM
动态随机存取存储器（`Dynamic Random Access Memory，DRAM`）是一种半导体存储器，主要的作用原理是利用电容内存储电荷的多寡来代表一个二进制比特（`bit`）是`1`还是`0`。由于在现实中晶体管会有漏电电流的现象，导致电容上所存储的电荷数量并不足以正确的判别数据，而导致数据毁损。因此对于`DRAM`来说，周期性地充电是一个无可避免的要件。由于这种需要定时刷新的特性，因此被称为“动态”存储器。相对来说，静态存储器（`SRAM`）只要存入数据后，纵使不刷新也不会丢失记忆

### SRAM
静态随机存取存储器（`Static Random-Access Memory，SRAM`）是随机存取存储器的一种。所谓的“静态”，是指这种存储器只要保持通电，里面储存的数据就可以恒常保持。相对之下，动态随机存取存储器（`DRAM`）里面所储存的数据就需要周期性地更新。然而，当电力供应停止时，`SRAM`储存的数据还是会消失（被称为`volatile memory`），这与在断电后还能储存资料的`ROM`或闪存是不同的。

### SDRAM
同步动态随机存取内存（`synchronous dynamic random-access memory`，简称`SDRAM`）是有一个同步接口的动态随机存取内存（`DRAM`）。通常`DRAM`是有一个异步接口的，这样它可以随时响应控制输入的变化。而`SDRAM`有一个同步接口，在响应控制输入前会等待一个时钟信号，这样就能和计算机的系统总线同步。时钟被用来驱动一个有限状态机，对进入的指令进行管线(`Pipeline`)操作。这使得`SDRAM`与没有同步接口的异步`DRAM`(`asynchronous DRAM`)相比，可以有一个更复杂的操作模式。

## 非易失型 ROM

* ROM
  * MASK ROM
  * PROM
    * OTPROM
    * EPROM
    * EEPROM
* FLASH
  * NOR FLASH
  * NAND FLASH

### ROM
只读存储器（Read-Only Memory，ROM）以非破坏性读出方式工作，只能读出无法写入信息。信息一旦写入后就固定下来，即使切断电源，信息也不会丢失，所以又称为固定存储器。ROM所存数据通常是装入整机前写入的，整机工作过程中只能读出，不像随机存储器能快速方便地改写存储内容。ROM所存数据稳定 ，断电后所存数据也不会改变，并且结构较简单，使用方便，因而常用于存储各种固定程序和数据。

### MASK ROM
掩膜只读存储器（`Mask ROM`）中存储的信息由生产厂家在掩膜工艺过程中“写入”。在制造过程中，将资料以一特制光罩（`Mask`）烧录于线路中，有时又称为“光罩式只读内存”（`Mask ROM`），此内存的制造成本较低，常用于电脑中的开机启动。其行线和列线的交点处都设置了MOS管，在制造时的最后一道掩膜工艺，按照规定的编码布局来控制`MOS`管是否与行线、列线相连。相连者定为1（或0），未连者为0（或1），这种存储器一旦由生产厂家制造完毕，用户就无法修改。

`MROM`的主要优点是存储内容固定，掉电后信息仍然存在,可靠性高。缺点是信息一次写入（制造）后就不能修改，很不灵活且生产周期长，用户与生产厂家之间的依赖性大。

### PROM - 可编程只读存储器
可编程只读存储器（`Programmable ROM，PROM`）允许用户通过专用的设备（编程器）一次性写入自己所需要的信息，其一般可编程一次，`PROM`存储器出厂时各个存储单元皆为1，或皆为0。用户使用时，再使用编程的方法使`PROM`存储所需要的数据。

`PROM`的种类很多，需要用电和光照的方法来编写与存放的程序和信息。但仅仅只能编写一次，第一次写入的信息就被永久性地保存起来。例如，双极性`PROM`有两种结构：一种是熔丝烧断型，一种是`PN`结击穿型。它们只能进行一次性改写，一旦编程完毕，其内容便是永久性的。由于可靠性差，又是一次性编程，较少使用。`PROM`中的程序和数据是由用户利用专用设备自行写入，一经写入无法更改，永久保存。`PROM`具有一定的灵活性，适合小批量生产，常用于工业控制机或电器中。

### OTPROM - 一次编程只读内存
一次编程只读内存（`One Time Programmable Read Only Memory，OTPROM`）之写入原理同`EPROM`，但是为了节省成本，编程写入之后就不再抹除，因此不设置透明窗。

### EPROM - 可编程可擦除只读存储器
可编程可擦除只读存储器（`Erasable Programmable Read Only Memory，EPROM`）可多次编程，是一种以读为主的可写可读的存储器。是一种便于用户根据需要来写入，并能把已写入的内容擦去后再改写的`ROM`。其存储的信息可以由用户自行加电编写，也可以利用紫外线光源或脉冲电流等方法先将原存的信息擦除，然后用写入器重新写入新的信息。 `EPROM`比`MROM`和`PROM`更方便、灵活、经济实惠。但是`EPROM`采用`MOS`管，速度较慢。

### EEPROM
电可擦可编程序只读存储器（`Electrically Erasable Programmable Read-Only Memory，EEPROM`）是一种随时可写入而无须擦除原先内容的存储器，其写操作比读操作时间要长得多，`EEPROM`把不易丢失数据和修改灵活的优点组合起来，修改时只需使用普通的控制、地址和数据总线。`EEPROM`运作原理类似`EPROM`，但抹除的方式是使用高电场来完成，因此不需要透明窗。 `EEPROM`比 `EPROM`贵，集成度低，成本较高，一般用于保存系统设置的参数、`IC`卡上存储信息、电视机或空调中的控制器。但由于其可以在线修改，所以可靠性不如 `EPROM`。


### FLASH
快擦除读写存储器(`Flash Memory`)是英特尔公司90年代中期发明的一种高密度、非易失性的读/写半导体存储器它既有`EEPROM`的特点，又有`RAM`的特点，是一种全新的存储结构，俗称快闪存储器。它在20世纪80年代中后期首次推出，快闪存储器的价格和功能介于 `EPROM`和`EEPROM`之间。与 `EEPROM`一样，快闪存储器使用电可擦技术，整个快闪存储器可以在一秒钟至几秒内被擦除，速度比 `EPROM`快得多。另外，它能擦除存储器中的某些块，而不是整块芯片。然而快闪存储器不提供字节级的擦除，与 `EPROM`一样，快闪存储器每位只使用一个晶体管，因此能获得与 `EPROM`一样的高密度(与 `EEPROM`相比较)。“闪存”芯片采用单一电源（3V或者5V）供电，擦除和编程所需的特殊电压由芯片内部产生，因此可以在线系统擦除与编程。“闪存”也是典型的非易失性存储器，在正常使用情况下，其浮置栅中所存电子可保存100年而不丢失。

### NOR FLASH 于 NAND FLASH
是市场上两种主要的非易失闪存技术之一。`Intel`于1988年首先开发出`NOR Flash` 技术，彻底改变了原先由`EPROM`(`Erasable Programmable Read-Only-Memory`电可编程序只读存储器)和`EEPROM`(电可擦只读存储器`Electrically Erasable Programmable Read - Only Memory`)一统天下的局面。紧接着，1989年，东芝公司发表了`NAND Flash` 结构，强调降低每比特的成本，有更高的性能，并且像磁盘一样可以通过接口轻松升级。`NOR Flash` 的特点是芯片内执行（`XIP ，eXecute In Place`），这样应用程序可以直接在`Flash`闪存内运行，不必再把代码读到系统`RAM`中。`NOR` 的传输效率很高，在1~4MB的小容量时具有很高的成本效益，但是很低的写入和擦除速度大大影响到它的性能。`NAND`的结构能提供极高的单元密度，可以达到高存储密度，并且写入和擦除的速度也很快。应用`NAND`的困难在于`Flash`的管理需要特殊的系统接口。通常读取`NOR`的速度比`NAND`稍快一些，而`NAND`的写入速度比`NOR`快很多，在设计中应该考虑这些情况。

关于更详细的 `NOR FLASH` 和 `NAND FLASH` 讲解请看下面这篇文章：
[https://www.cnblogs.com/iriczhao/p/12128451.html](https://www.cnblogs.com/iriczhao/p/12128451.html)