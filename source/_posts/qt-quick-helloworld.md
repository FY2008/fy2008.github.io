---
title: 第一个 Qt Quick 程序
date: 2021-02-02 18:46:17
tags:
- Qt
- Qt Quick
- QML
category:
- 计算机
index_img:
banner_img:
---

用 Qt Create 创建第一个 Qt Quick 程序。

## 新建 Qt Quick 程序
新建对话框
![new-qt-quick](/img/new-qt-quick.png)

新建完成后的工程结构

![qt-quick-project-struct](/img/qt-quick-project-struct.png)

## main.qml
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

main.cpp
```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[])
{
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);

    QGuiApplication app(argc, argv);

    QQmlApplicationEngine engine;

    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));

    return app.exec();
}
```

运行结果

![qt-quick-helloworld-img](/img/qt-quick-helloworld-img.png)