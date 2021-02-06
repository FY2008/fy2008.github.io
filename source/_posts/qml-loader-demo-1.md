---
title: QML Loader 简单使用
date: 2021-02-05 13:36:37
tags:
- Qt
- QML
- Loader
category:
- 计算机
index_img: /img/13.jpg
banner_img: /img/13.jpg
---

`Loader` 是用来动态加载 `QML` 组件的。我们可以把 `Loader` 作为占位符使用，在需要显示某个元素时，才使用 `Loader` 把它加载进来。

`Loader` 可以使用其 `source` 属性加载一个 `QML` 文档，也可以通过其 `sourceComponent` 属性加载一个`Component` 对象。当你需要延迟一些对象直到真正需要才创建它们时，`Loader` 非常有用。当 `Loader` 的 `source` 或 `sourceComponent` 属性发生变化时，它之前加载的 `Component` 会自动销毁，新对象会被加载。将 `source` 设置为一个字符串或将 `sourceComponent` 设置为 `undefined`，将会销毁当前加载的对象，相关的资源也会被释放，而 `Loader` 对象则变成一个空对象。

下面展示一个例子：

```qml
import QtQuick 2.12
import QtQuick.Window 2.12

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Rectangle {
        anchors.fill: parent
        color: "#C0C0C0"

        Text {
            id: coloredText
            anchors.horizontalCenter: parent.horizontalCenter
            anchors.top: parent.top
            anchors.topMargin: 4
            text: qsTr("Hello World!")
            font.pixelSize: 32
        }

        Component {
            id: colorComponent
            Rectangle {
                id: colorPicker
                width: 50
                height: 30
                signal colorPicked(color clr)
                property Item loader
                border.width: focus ? 2 : 0
                border.color: focus ? "#90D750" : "#808080"
                MouseArea {
                    anchors.fill: parent
                    onClicked: {
                        colorPicker.colorPicked(colorPicker.color)
                        loader.focus = true
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
            }
        }

        Loader {
            id: redLoader
            width: 80
            height: 60
            focus: true
            anchors.left: parent.left
            anchors.leftMargin: 4
            anchors.bottom: parent.bottom
            anchors.bottomMargin: 4
            sourceComponent: colorComponent
            KeyNavigation.right: blueLoader
            KeyNavigation.tab: blueLoader
            onLoaded: {
                item.color = "red"
                item.focus = true
                item.loader = redLoader
            }
            onFocusChanged: {
                item.focus = focus
            }
        }

        Loader {
            id: blueLoader
            anchors.left: redLoader.right
            anchors.leftMargin: 4
            anchors.bottom: parent.bottom
            anchors.bottomMargin: 4
            sourceComponent: colorComponent
            KeyNavigation.left: redLoader
            KeyNavigation.tab: redLoader
            onLoaded: {
                item.color = "blue"
                item.loader = blueLoader
            }
            onFocusChanged: {
                item.focus = focus
            }
        }

        Connections {
            target: redLoader.item
            onColorPicked: {
                coloredText.color = clr
            }
        }

        Connections {
            target: blueLoader.item
            onColorPicked: {
                coloredText.color = clr
            }
        }
    }
}

```

![qml-loader](/img/qml-loader.png)