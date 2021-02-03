---
title: CMake 入门
date: 2021-01-21 09:29:20
tags:
category:
index_img:
banner_img:
---

## 概述
> CMake is an open-source, cross-platform family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and generate native makefiles and workspaces that can be used in the compiler environment of your choice. The suite of CMake tools were created by Kitware in response to the need for a powerful, cross-platform build environment for open-source projects such as ITK and VTK. [https://cmake.org/](https://cmake.org/)

## 入门
我们先用 `CMake` 来构建一个简单的 `c` 程序。

创建 `helloworld` 文件夹，然后进入这个文件夹，在里边创建一下文件：
.
├── CMakeLists.txt
└── main.c

`CMakeLists.txt` 是 `CMake` 的配置文件。
`main.c` 是 `c` 程序的源文件。

main.c
```c
#include <stdio.h>

int main(void)
{
    printf("Hello,World!\n");
    return 0;
}
```

CMakeLists.txt
```cmake
# cmake 最低版本
cmake_minimum_required(VERSION 3.5)

# 目标程序
project(zsf_Hello C)

# 编译源码生产目标
add_executable(zsf_Hello main.c)
```

执行 `cmake .. ` 进行构建，执行 `make` 构建 生成后的Makefile 文件。

## 优化目录结构
新的目录结构如下：
```cmake
.
├── CMakeLists.txt
├── build
├── lib
│   ├── CMakeLists.txt
│   ├── zsf_math.c
│   └── zsf_math.h
└── src
    ├── CMakeLists.txt
    └── main.c
```

### CMakeLists.txt
```
# 顶层 CMakeLists.txt

# cmake 最低版本
cmake_minimum_required(VERSION 3.5)

# 目标程序
project(zsf_Hello C)

# 添加子目录
add_subdirectory(./lib)
add_subdirectory(./src)

#启动对C++14标准的支持
#set(CMAKE_CXX_STANDARD 14)
# 显式要求指明支持C++标准
#set(CMAKE_CXX_STANDARD_REQUIRED True)

# 启动对C11的支持
set(CMAKE_C_STANDARD 11)
# 显式要求指明支持C标准
set(CMAKE_C_STANDARD_REQUIRED True)

```

### src/CMakeLists.txt
```cmake
# 源文件 CMakeLists.txt

# 包含库路径
include_directories(${PROJECT_SOURCE_DIR}/lib)

# 源文件入口路径
aux_source_directory(./ DIR_SRC)

# 设置可执行文件的输出路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

# 编译源码生产目标
add_executable(zsf_Hello main.c)

# 链接库
target_link_libraries(zsf_Hello zsfMath)
```

### lib/CMakeLists.txt
```cmake
# 库文件 CMakeLists.txt

# 包含源文件路径
aux_source_directory(. DIR_SRC)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# 添加库
add_library(zsfMath STATIC ${DIR_SRC})
```

### src/main.c
```c
#include <stdio.h>
#include "zsf_math.h"

int main(void)
{
    printf("Hello,World!\n");
    printf("结果: %d\n", add(10, 20));
    return 0;
}
```

### lib/zsf_math.h
```c
#ifndef _ZSF_MATH_
#define _ZSF_MATH_

int add(int a, int b);

#endif
```

### lib/zsf_math.c
```c
#include "zsf_math.h"

int add(int a, int b){
    return a + b;
}
```

## 判断语句与循环语句
### if 判断语句
CMake 支持 if 语句。非零为真
例子1：
```cmake
set(var 1)
if(var)
    message(${var})
endif(var)
```

例子2
```cmake
set(var 1)
if(var)
    message(${var})
else(var)
    message(${var})
endif(var)
```

例子3
```cmake
# if 语句1
set(Mobile "小米手机")
if(Mobile STREQUAL "苹果手机")
    MESSAGE("我的手机是苹果的")
elseif(Mobile STREQUAL "小米手机")
    MESSAGE("我的手机是小米的")
else(Mobile)
    MESSAGE("......else......")
endif(Mobile)
```



## 变量列表
* CMAKE_INSTALL_PREFIX
* 

## 命令列表
1. cmake_minimum_required(VERSION 3.5)
2. project(zsf_Hello C)
3. add_executable(zsf_Hello main.c)
4. add_subdirectory(./lib)
5. add_library(zsfMath STATIC ${DIR_SRC})
6. add_definitions(-DFOO -DTEST)
7. include_directories(${PROJECT_SOURCE_DIR}/lib)
8. aux_source_directory(. DIR_SRC)
9. set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
10. set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
11. file()
12. set_target_properties()
13. message([STATUS|WARNING|AUTHOR_WARNING|FATAL_ERROR|SEND_ERROR]
  "message to display" ...)
14. INSTALL()


### set_property()
```
set_property(<GLOBAL                      |
              DIRECTORY [<dir>]           |
              TARGET    [<target1> ...]   |
              SOURCE    [<src1> ...]      |
              INSTALL   [<file1> ...]     |
              TEST      [<test1> ...]     |
              CACHE     [<entry1> ...]    >
             [APPEND] [APPEND_STRING]
             PROPERTY <name> [value1 ...])
```

### set_target_properties()
`SET_TARGET_PROPERTIES`，其基本语法是：
```
SET_TARGET_PROPERTIES(target1 target2 ...
PROPERTIES prop1 value1
prop2 value2 ...)
```

这条指令可以用来设置输出的名称，对于动态库，还可以用来指定动态库版本和API版本。
在本例中，我们需要作的是向`lib/CMakeLists.txt`中添加一条：
`SET_TARGET_PROPERTIES(hello_static PROPERTIES OUTPUT_NAME "hello")`
这样，我们就可以同时得到`libhello.so/libhello.a`两个库了。

### GET_TARGET_PROPERTY(VAR target property)
`GET_TARGET_PROPERTY(VAR target property)`
具体用法如下例，我们向`lib/CMakeListst.txt`中添加：
```
GET_TARGET_PROPERTY(OUTPUT_VALUE hello_static OUTPUT_NAME)
MESSAGE(STATUS “This is the hello_static
OUTPUT_NAME:”${OUTPUT_VALUE})
```
如果没有这个属性定义，则返回`NOTFOUND`.

###  message()
函数原型
 message( [STATUS|WARNING|AUTHOR_WARNING|FATAL_ERROR|SEND_ERROR]
  "message to display" ...)

```
(无) = 重要信息;
STATUS = 非重要信息;
WARNING = CMake 警告，会继续执行;
AUTHOR_WARNING = CMake 警告（dev），会继续执行;
SEND_ERROR = CMake 错误，继续执行，但是会跳过生成的步骤;
FATAL_ERROR = CMake 错误，终止所有处理过程
```
例子：
```cmake
#Message
message(STATUS "BINARY 路径" ${PROJECT_BINARY_DIR})
message(STATUS "SOURCE 路径" ${PROJECT_SOURCE_DIR})
```

### 安装命令 INSTALL
```
INSTALL(TARGETS targets...
[[ARCHIVE|LIBRARY|RUNTIME]
[DESTINATION <dir>]
[PERMISSIONS permissions...]
[CONFIGURATIONS
[Debug|Release|...]]
[COMPONENT <component>]
[OPTIONAL]
] [...])
```
参数中的`TARGETS`后面跟的就是我们通过`ADD_EXECUTABLE`或者`ADD_LIBRARY`定义的
目标文件，可能是可执行二进制、动态库、静态库。

目标类型也就相对应的有三种，ARCHIVE特指静态库，LIBRARY特指动态库，RUNTIME
特指可执行目标二进制。

`DESTINATION`定义了安装的路径，如果路径以/开头，那么指的是绝对路径，这时候
`CMAKE_INSTALL_PREFIX`其实就无效了。如果你希望使用`CMAKE_INSTALL_PREFIX`来
定义安装路径，就要写成相对路径，即不要以`/`开头，那么安装后的路径就是
`${CMAKE_INSTALL_PREFIX}/<DESTINATION定义的路径>`

{% label primary @举个简单的例子： %}
```
INSTALL(TARGETS myrun mylib mystaticlib
RUNTIME DESTINATION bin
LIBRARY DESTINATION lib
ARCHIVE DESTINATION libstatic
)
```
上面的例子会将：
可执行二进制`myrun`安装到`${CMAKE_INSTALL_PREFIX}/bin`目录
动态库`libmylib`安装到`${CMAKE_INSTALL_PREFIX}/lib`目录
静态库`libmystaticlib`安装到`${CMAKE_INSTALL_PREFIX}/libstatic`目录

特别注意的是你不需要关心`TARGETS`具体生成的路径，只需要写上`TARGETS`名称就可以
了。
普

{% label success @普通文件的安装： %}
```
INSTALL(FILES files... DESTINATION <dir>
[PERMISSIONS permissions...]
[CONFIGURATIONS [Debug|Release|...]]
[COMPONENT <component>]
[RENAME <name>] [OPTIONAL])
```

可用于安装一般文件，并可以指定访问权限，文件名是此指令所在路径下的相对路径。如果
默认不定义权限`PERMISSIONS`，安装后的权限为：
`OWNER_WRITE`, `OWNER_READ`, `GROUP_READ`,和`WORLD_READ`，即`644`权限。

{% label success @非目标文件的可执行程序安装(比如脚本之类)： %}
```
INSTALL(PROGRAMS files... DESTINATION <dir>
[PERMISSIONS permissions...]
[CONFIGURATIONS [Debug|Release|...]]
[COMPONENT <component>]
[RENAME <name>] [OPTIONAL])
```
跟上面的`FILES`指令使用方法一样，唯一的不同是安装后权限为:
`OWNER_EXECUTE`, `GROUP_EXECUTE`, 和`WORLD_EXECUTE`，即`755`权限

{% label success @目录的安装： %}
```
INSTALL(DIRECTORY dirs... DESTINATION <dir>
[FILE_PERMISSIONS permissions...]
[DIRECTORY_PERMISSIONS permissions...]
[USE_SOURCE_PERMISSIONS]
[CONFIGURATIONS [Debug|Release|...]]
[COMPONENT <component>]
[[PATTERN <pattern> | REGEX <regex>]
[EXCLUDE] [PERMISSIONS permissions...]] [...])
```
这里主要介绍其中的`DIRECTORY`、`PATTERN`以及`PERMISSIONS`参数。
`DIRECTORY`后面连接的是所在`Source`目录的相对路径，但务必注意：
`abc`和`abc/`有很大的区别。
如果目录名不以`/`结尾，那么这个目录将被安装为目标路径下的`abc`，如果目录名以`/`结尾，
代表将这个目录中的内容安装到目标路径，但不包括这个目录本身。
`PATTERN`用于使用正则表达式进行过滤，`PERMISSIONS`用于指定`PATTERN`过滤后的文件
权限。

我们来看一个例子:
```
INSTALL(DIRECTORY icons scripts/ DESTINATION share/myproj
PATTERN "CVS" EXCLUDE
PATTERN "scripts/*"
PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ
GROUP_EXECUTE GROUP_READ)
```
这条指令的执行结果是：
将`icons`目录安装到 `<prefix>/share/myproj`，将`scripts/`中的内容安装到
`<prefix>/share/myproj`
不包含目录名为`CVS`的目录，对于`scripts/*`文件指定权限为 `OWNER_EXECUTE
OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ.`

安装时CMAKE脚本的执行：
`INSTALL([[SCRIPT <file>] [CODE <code>]] [...])`
`SCRIPT`参数用于在安装时调用cmake脚本文件（也就是`<abc>.cmake`文件）
`CODE`参数用于执行`CMAKE`指令，必须以双引号括起来。比如：
`INSTALL(CODE "MESSAGE(\"Sample install message.\")")`

安装还有几个被标记为过时的指令，比如`INSTALL_FILES`等，这些指令已经不再推荐使
用，所以，这里就不再赘述了。

## 动态库版本号
按照规则，动态库是应该包含一个版本号的，我们可以看一下系统的动态库，一般情况是
```
libhello.so.1.2
libhello.so ->libhello.so.1
libhello.so.1->libhello.so.1.2
```
为了实现动态库版本号，我们仍然需要使用`SET_TARGET_PROPERTIES`指令。
具体使用方法如下：
`SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)`
`VERSION`指代动态库版本，`SOVERSION`指代`API`版本。
将上述指令加入`lib/CMakeLists.txt`中，重新构建看看结果。
在`build/lib`目录会生成：
```
libhello.so.1.2
libhello.so.1->libhello.so.1.2
libhello.so ->libhello.so.1
```
终端实例
```bash
zsf99@DESKTOP-N25R117:~/CExample/cmake_demo/libhello/build/lib$ ll
total 44
drwxr-xr-x 3 zsf90 zsf90  4096 Jan 22 16:26 ./
drwxr-xr-x 4 zsf90 zsf90  4096 Jan 22 16:26 ../
drwxr-xr-x 5 zsf90 zsf90  4096 Jan 22 16:26 CMakeFiles/
-rw-r--r-- 1 zsf90 zsf90  6600 Jan 22 16:07 Makefile
-rw-r--r-- 1 zsf90 zsf90  1134 Jan 22 15:12 cmake_install.cmake
-rw-r--r-- 1 zsf90 zsf90  1822 Jan 22 16:26 libhello.a
lrwxrwxrwx 1 zsf90 zsf90    13 Jan 22 16:26 libhello.so -> libhello.so.1*
lrwxrwxrwx 1 zsf90 zsf90    15 Jan 22 16:26 libhello.so.1 -> libhello.so.1.2*
-rwxr-xr-x 1 zsf90 zsf90 16200 Jan 22 16:26 libhello.so.1.2*
```

## CMake模块
