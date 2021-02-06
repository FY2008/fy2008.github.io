---
title: QML 基础
date: 2021-02-02 19:35:32
tags:
- Qt
- QML
category:
- 计算机
index_img: /img/12.jpg
banner_img: /img/12.jpg
---

`QML` 是一种类似 `CSS` 与 `JavaScript` 相结合的描述用户界面的语言。

`QML` 实现并扩展了 `ECMAScript`，是一备战说明性语言，用来描述基于 `Qt` 对象系统的用户界面。`QML` 提供了高可读性的，声明式的、类 `CSS` 的语法，支持结合动态属性绑定的 `ECMAScript` 表达式。

## 对象
`QML` 文件的后缀是 `.qml`,其实就是文本文件。简单的 QML 文件：
```qml
import QtQuick 2.12
import QtQuick.Window 2.12

Window {
    id: root
    visible: true
    width: 640
    height: 480
    title: qsTr("Hello World")
    minimumWidth: 400   // 窗口最小宽度
    minimumHeight: 250  // 窗口最小高度

    Text {
        id: textHello
        anchors.centerIn: parent
        text: qsTr("Hello,World")
        font.pointSize: 24
        font.bold: true
        color: "#008787"
    }
}
```

QML 通过 `import xxx` 方式导入相关模块。

Rectangle {} 语句，定以了一个类型为 Rectangle 的对象。对象要用一对花括号来描述，花括号前要写上对象的类型名字（类名）。

## 表达式
QML 支持 ECMAScript 表达式。你可以在样初始化 Rectangle 对象的宽、高属性：
```qml
Rectangle {
    width: 22*10
    height: 2*4
    color: "#ccc"
}
```

## 引用对象
QML中用对象的 id 值来引用一个对象。
```qml
Rectangle {
    width: 320
    height: 480

    Button {
        id: openFile
        text: "打开"
    }

    Button {
        id: quit
        text: "退出"
        anchors.left: openFile.right
    }
}
```

`anchors.left: openFile.right` 这句就是使用引用的例子

## 注释
`QML` 中的注释方式种 c`++` 中的一样。

## 属性
其实，`QML` 中的属性，对应于我们非常熟悉的 `C++`、`Java` 中类的成员变量...

### 属性命名
属性名的首字母一般以小写开始，如果属性名以多个单词表示，那么第二个及以后的单词，首字母大写。这也是驼峰命名法。

### 属性的类型
可以在 `QML` 文档中使用的类型大概有三类：
* 由 `QML` 语言本身提供的类型。
* 由 `QML` 模块（比如 `Qt` `Quick`）提供的类型。
* 导出到 `QML` 环境中的 `C++` 类型。

#### 基本类型
`QML` 支持的基本类型包括 `int`、`real`、`double`、`bool`、`string`、`color`、`list`、`font`等。

请使用 `Qt`帮助的索引模式，以“qml basic types“为关键字检索，找到 `QML Basic Types` 页面来查看完整的类型列表和每种类型的详情。

`Qt` 的 `QML` 模块还为`QML` 引入了很多Qt相关的类型，如：`Qt`、`QtObject`、`Component`、`Connections`、`Binding`等，请使用 `Qt` 帮助检索 “qt qml qml types“来了解。

#### id属性
一个对象的 `id` 属性是唯一的，在同一个 `QML` 文件中不同对象的 `id` 属性的值不能重复。

#### 列表属性
`QML` 对象的列表属性（类型是 `list`）类似于下面这样：
```qml
Item {
    children: [
        Image {},
        Text {}
    ]
}
```

列表是包含在方括号内，以逗号分隔的多个元素的集合。
其实列表和 `ECMAScript` 的数组（`Array`）是类似的，其访问方式也一样：
* 可以用[value1,value2,...,valueN]这种形式给 `list` 对象赋值
* `length` 属性提供了列表内元素的个数
* 列表内的元素通过数组下标来放问([index])

值得注意的是，列表内只能包含`QML`对象，不能包含任何基本类型的字面量（如：`9`、`true`，如果非要包含，需要使用 `var` 变量）。

```qml
Item {
    children: [
        Text {
            text: "textOne"
        }
        Text {
            text: "textTwo"
        }
    ]

    Component.onCompleted: {
        for (var i = 0; i < children.length; i++) {
            console.log("text of label ", i, " : ", children[i].text)
        }
    }
}
```

如果一个列表内只的一个元素，也可以省略方括号，如下所示：
```qml
Item {
    children: Image {}
}
```

#### 信号处理器
信号处理器，其实等价于 `Qt` 中的槽。它的名字还有点特别，一般是 on<Signal>这种形式。比如 `Qt Quick`中的`Button`元素有一个信号 `clicked()`，那么你可能会写出这样的代码：
```qml
Rectangle {
    width: 320
    height: 480

    Button {
        id: quit
        text: "退出"
        anchors.left: parent.left
        anchors.leftMargin: 4
        anchors.bottom: parent.bottom
        anchors.bottomMargin: 4
        onClicked: {
            Qt.quit()
        }
    }
}
```

你看到了，当信号是 `clicked()` 时，信号处理器就命名为 `onClicked`。就这么简单，以 `on` 起始后跟信号名字（第一个字母大写）。

#### 分组属性
在某些情况下使用一个“.“符号或分组符号将相关的属性形成一个逻辑组。有时我们给分组属性赋值是一个个来的，类似于这样：
```qml
Text {
    font.pixelSize: 18
    font.bold: true
}
```
其实下面这样的写法在形式上更贴合分组的含义：
```qml
Text{
    font {
        pixelSize: 12
        bold: true
    }
}
```

其实中以这么理解，`font` 属性的类型本身是一个对象，这个对象又有 `pixelSize`、`bold`、`italic`、`underline` 等属性。对于类型为对象的属性值，可以使用“.”操作符展开对象的每一个成员对其赋值，也可以通过分组符号（一对花括号）把要赋值的成员放在一起给它们赋值。对于后者，其形式和对象的定义一样了，起码看起来没有区别。所以，又可以这么理解上面的示例：`Text` 对象内聚合了 `font` 对象。

#### 附加属性
在`QML` 语言的语法中，有一个附加属性（`attached properties`) 的概念，这是附加到一个对象上的额外的属性。

举个例子，下面的 `Item` 对象使用了附加属性：
```qml
import QtQuick 2.2

Item {
    width: 100
    height: 100

    focus: true
    Keys.enabled: false
}
```
`Item` 对象设置 `Keys.enabled` 为 `false`，`Keys` 就是 `Qt Quick` 提供的供 `Item` 处理按键事件的附加属性。与附加属性相似的概念还有附加信号处理器。

文章内容来自：《Qt Quick 核心编程》