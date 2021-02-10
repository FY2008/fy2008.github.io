---
title: Qt 设置程序图标
date: 2021-02-09 11:01:42
tags:
- Qt
- 程序图标
category:
- 计算机
index_img: /img/qt-icon-app-win.png
banner_img: /img/3.jpg
---

Qt 设置程序图标和窗口图标的方法以及注意事项：

## 设置窗口图标
设置窗口图标的方法很简单，通过 `setWindowIcon(QIcon(":/images/logo.ico"))` 方法即可，这里要注意的是，`QIcon()` 参数中的路径不能是 Url 形式的（qrc:/images/logo.ico），通过在资源文件上鼠标右击选择复制路径获取路径。

![qt-icon-path](/img/qt-icon-path.png)

![qt-icon-window](/img/qt-icon-window.png)

## 设置应用程序图标
通过应用程序图标就是生成的 .exe 文件的图标，方法是在Qt 工程的 SerialTool.pro 文件中添加 RC_ICONS = logo.ico ，添加应用程序的图标。

通过添加 RC_ICONS = logo.ico 方式添加的应用程序图标，在没有设置程序的窗口图标时，同样也会改变窗口的图标，通过同时设置窗口图标和应用程序图标可以给程序添加窗口与应用程序不一样的图标。

## SerialTool.pro
```qmake
QT       += core gui

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

CONFIG += c++11

# You can make your code fail to compile if it uses deprecated APIs.
# In order to do so, uncomment the following line.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0

SOURCES += \
    main.cpp \
    mainwindow.cpp

HEADERS += \
    mainwindow.h

FORMS += \
    mainwindow.ui

# Default rules for deployment.
qnx: target.path = /tmp/$${TARGET}/bin
else: unix:!android: target.path = /opt/$${TARGET}/bin
!isEmpty(target.path): INSTALLS += target

RESOURCES += \
    Resource.qrc

RC_ICONS = images/logo.ico

```

![qt-icon-app-win](/img/qt-icon-app-win.png)