---
title: QML ECMAScript 基础
date: 2021-02-03 11:20:05
tags:
- Qt
- QML
- ECMAScript
category:
- 计算机
index_img:
banner_img:
---

测试`ECMAScript`的代码可发放在 Component.onCompleted: {}
```qml
import QtQuick 2.12

Rectangle {
    Component.onCompleted: {
        // 这里放测试代码
    }
}
```
## 语法

如果你熟悉 `C`、`C++`、或者`Java`，就会发现 `ECMAScript` 的语法很容易掌握。

### 区分大小写
与`C++`一样，变量、函数名、运算符以及其他一切东西都是区分大小的。

### 弱类型
与`C++`不同，`ECMAScript` 中的变量没有特定的类型，定义变量时只用 `var` 运算符，可以将它初始化为任意的值，你可以随时改变变量所存储的数据类型。示例：
```javascript
var background = "white"
var i = 0
var children = new Array()
var focus = true
```
### 语句后的分号可有可无
C、C++、Java等语言都要求每条语句以分号（;）结束。ECMAScript 则允许开发者自行决定是否以分号结束一行代码。

## 注释
ECMAScript 借用了 C、Java等语言的注释语法。

## 关键字
与C++类似，关键字是保留的，不能用作变量名或函数名。下面是 ECMAScript 中的关键字列表：
* break
* case
* catch
* continue
* default
* delete
* do
* else
* finally
* for
* function
* if
* in
* instanceof
* new
* return
* switch
* this
* throw
* try
* typeof
* var
* void
* while
* with

## 保留字
保留字是留给将来的，可能会作为关键字。保留字也不能用作变量名或函数名。ECMAScript 中有下列保留字：
* abstract
* boolean
* byte
* char
* class
* const
* debugger
* double
* enum
* export
* extends
* final
* float
* goto
* implements
* import
* int
* interface
* long
* native
* package
* private
* protected
* public
* short
* static
* super
* synchronized
* throws
* transient
* volatile

