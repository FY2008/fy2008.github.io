---
title: 在 ECMAScript 中动态创建对象
date: 2021-02-05 21:19:01
tags:
- Qt
- QML
- 动态创建对象
- ECMAScript
category:
- 计算机
index_img: /img/14.jpg
banner_img: /img/14.jpg
---

`QML` 支持在 `ECMAScript` 中动态创建对象。这对于延迟对象的创建、缩短应用的启动时间都是有帮助的。同时这种机制也使得我们可以根据用户的输入或者某些事件动态地将可见元素添加到应用场景中。

在 `ECMAScript` 中，有两种方式可以动态地创建对象：
* 使用 `Qt.createComponent()` 动态地创建一个组件对象，然后使用 `Component` 的 `createObject()` 方法狼子野心建对象。
* 使用 `Qt.createQmlObject()` 从一个 `QML` 字符串直接创建一个对象。

## 从组件文件动态创建 Component
`Qt` 对象的 `createComponent()` 方法可以根据 `QML` 文件动态地创建一个组件。函数原型如下：
```
object createComponent(url, mode, parent)
```
第一个参数 `url` 指向 `QML` 文档的本地路经或网络地址；第二个参数 `mode` 指定创建组件的模式，可以是 `Component.PreferSynchronous`（优先使用同步模式）或 `Component.Asynchronous`（异步模式），忽略 `mode` 参数时，默认使用 `PreferSynchronous` `模式；parent` 参数指定组件的父对象。

示例程序

### main.qml
```qml
import QtQuick 2.12
import QtQuick.Window 2.12
import QtQuick.Controls 2.12

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("使用 Qt.createComponent() 动态创建组件实例")

    Rectangle {
        id: rootItem
        anchors.fill: parent
        property var count: 0
        property Component component: null

        Text {
            id: coloredText
            text: qsTr("Hello World!")
            anchors.centerIn: parent
            font.pixelSize: 24
        }

        function changeTextColor(clr){
            coloredText.color = clr
        }

        function createColorPicker_s(clr){
            if(rootItem.component == null){
                rootItem.component = Qt.createComponent("ColorPicker.qml")
            }
            var colorPicker
            if(rootItem.component.status == Component.Ready){
                colorPicker = rootItem.component.createObject(rootItem,
                    {"color" : clr, "x" : rootItem.count * 55, "y" : 10})
                colorPicker.colorPicked.connect(rootItem.changeTextColor)
            }
            rootItem.count++
        }

        Button {
            id: add
            text: "add"
            anchors.left: parent.left
            anchors.leftMargin: 4
            anchors.bottom: parent.bottom
            anchors.bottomMargin: 4
            onClicked: {
                console.debug("Debug Button Clicked...")
                rootItem.createColorPicker_s(Qt.rgba(Math.random(), Math.random(), Math.random(), 1))
            }
        }

    } /* END Rectangle */
}
```

![qt_create_component_1.gif](/img/qt_create_component_1.gif)

## 从 QML 字符串动态创建 Component
如果你的软件，需要在运行过程中根据应用的状态适时地生成用于描述对象的 QML 字符串，进而根据这个 QML 字符串创建对象，那么可以像下面这样创建对象：
```qml
var newObject = Qt.createQmlObject('import QtQuick 2.12;
    Rectangle {color: "red"; width: 20; height: 20}', parentItem, "dynamicSnippet1")
```
createQmlObject 方法的第一个参数是要创建对象的 QML 字符串，就像一个 QML 文档一样，你需要导入你用到的所有类型和模块；第二个参数用于指定要创建的对象的父对象；第三个参数用于给新创建的对象关联一个文件路径，主要用于报告错误。