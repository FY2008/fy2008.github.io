---
title: 销毁动态创建的对象
date: 2021-02-06 21:50:27
tags:
- Qt
- QML
category:
- 计算机
index_img: /img/15.jpg
banner_img: /img/15.jpg
---

有些软件，在不需要一个动态创建的 `QML` 对象时，仅仅是把它的 `visible` 属性设置为 `false` 或者把 `opacity` 属性设置为 `0`，而不是删除这个对象。如果动态创建的对很多，无用的对象都这么处理而不直接删除，那会给软件带来比较大的性能问题，比如内存占用增多，运行速度变慢等。所以呢，动态创建的对象，不再使用时，最好把它删除。

我们这里说的动态创建的对象，特指使用 `Qt.createComponent()` 或 `Qt.createQmlObject()` 方法创建的对象，而使用 `Loader` 创建的对象，应当通过将 `source` 设置为空串或将 `sourceComponent` 设置为 undefined 触发 `Loader` 销毁它们。

要删除一个对象，可以调用其 `destroy()` 方法。`destroy()` 方法有一个可选的参数，指定延迟多少毫秒再删除这个对象，其默认值为 0。`destroy()` 方法有点儿像 `Qt C++`中 `QObject` 的 `deleteLater()` 方法，即便你设定延迟为 0 去调用它，对象也并不会立即删除，`QML` 引擎会在当前代码块执行结束后的某个合适的时刻删除它们。所以，即便你在一个对象内部调用 `destroy()` 方法也是安全的。

## 代码1
main.qml
```qml
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.12

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("销毁动态创建的组件")

    Rectangle {
        id: rootItem
        anchors.fill: parent
        property var count: 0
        property Component component: null

        Text {
            id: coloredText
            text: qsTr("Hello World")
            anchors.centerIn: parent
            font.pixelSize: 24
        }

        function changeTextColor(clr){
            coloredText.color = clr
        }

        function createColorPicker(clr){
            if(rootItem.component == null){
                rootItem.component = Qt.createComponent("ColorPicker.qml")
            }
            var colorPicker
            if(rootItem.component.status == Component.Ready){
                colorPicker = rootItem.component.createObject(rootItem,
                    {"color": clr, "x": rootItem.count * 55, "y": 10})
                colorPicker.colorPicked.connect(rootItem.changeTextColor)

                if(rootItem.count % 2 == 1){
                    colorPicker.destroy(1000)
                }
            }
            rootItem.count++
        }

        Button {
            id: add
            text: "add";
            anchors.left: parent.left;
            anchors.leftMargin: 4
            anchors.bottom: parent.bottom;
            anchors.bottomMargin: 4
            onClicked: {
                rootItem.createColorPicker(Qt.rgba(Math.random(), Math.random(), Math.random(), 1))
            }
        }
    }
}
```

ColorPicker.qml
```qml
import QtQuick 2.0

Rectangle {
    id: colorPicker
    width: 50
    height: 30
    signal colorPicked(color clr)

    function configureBorder(){
        colorPicker.border.width = colorPicker.focus ? 2 : 0
        colorPicker.border.color = colorPicker.focus ? "#90D750" : "#808080"
    }

    MouseArea {
        anchors.fill: parent
        onClicked: {
            colorPicker.colorPicked(colorPicker.color)
            mouse.accepted = true
            colorPicker.focus = true
        }
    }

    Keys.onReturnPressed: {
        colorPicker.colorPicked(colorPicker.color)
        event.accepted = true
    }

    Keys.onSpacePressed: {
        colorPicker.colorPicked(colorPicker.color)
        event.accepted = true
    }

    onFocusChanged: {
        configureBorder()
    }

    Component.onCompleted: {
        configureBorder()
    }
}
```

### 演示1

![destroy-component.gif](/img/destroy-component.gif)

以上代码实现了隔一个删除一个颜色选取块。

## 代码2
main.qml
```qml
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.12

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Rectangle {
        id: rootItem
        anchors.fill: parent
        property var count: 0;
        property Component component: null;
        property var dynamicObjects: new Array();

        Text {
            id: coloredText;
            text: qsTr("Hello World")
            anchors.centerIn: parent
            font.pixelSize: 24
        }

        function changeTextColor(clr){
            coloredText.color = clr
        }

        function createColorPicker(clr){
            if(rootItem.component == null){
                rootItem.component = Qt.createComponent("ColorPicker.qml")
            }
            var colorPicker
            if(rootItem.component.status == Component.Ready){
                colorPicker = rootItem.component.createObject(rootItem,
                    {
                        "color": clr,
                        "x": rootItem.dynamicObjects.length * 55,
                        "y": 10
                    })
                colorPicker.colorPicked.connect(rootItem.changeTextColor)
                rootItem.dynamicObjects[rootItem.dynamicObjects.length] = colorPicker
            }
        }

        Button {
            id: add
            text: "add"
            anchors.left: parent.left
            anchors.leftMargin: 4
            anchors.bottom: parent.bottom
            anchors.bottomMargin: 4
            onClicked: {
                rootItem.createColorPicker(Qt.rgba(Math.random(), Math.random(), Math.random(), 1))
            }
        }

        Button {
            id: del
            text: "del"
            anchors.left: add.right
            anchors.leftMargin: 4
            anchors.bottom: add.bottom
            onClicked: {
                if(rootItem.dynamicObjects.length > 0){
                    var deleted = rootItem.dynamicObjects.splice(-1, 1)
                    deleted[0].destroy()
                }
            }
        }
    }
}
```

### 演示2
![destroy-component2.gif](/img/destroy-component2.gif)

文章来自《Qt Quick核心编程.pdf》