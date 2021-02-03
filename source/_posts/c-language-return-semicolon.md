---
title: C语言中"return;"的使用介绍
date: 2021-01-06 14:36:20
tags: C语言
category: 计算机
index_img:
banner_img: /img/6.jpg
---

在 `C语言`中 `return;` 用于从返回值为 `void` 的函数中强制结束函数，把程序的控制权交给函数的调用者。

```c
#include <stdio.h>


void demo(int a){
    if (a < 0) {
        printf("a 小于0\n");
        return; // 直接跳出函数
    }

    printf("测试能否执行到这里\n"); // 如果 a < 0, 则这条语句不会被执行到
}

int main(void)
{
    demo(-5);
    //demo(5);
    return 0;
}
```

`return;` 用在返回值为 `void` 的函数中的场景很多，所有有必要掌握使用它。
