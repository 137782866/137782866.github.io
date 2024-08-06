---
title: "main函数的参数和返回值"
date: 2024-08-05T11:39:49+08:00
lastmod: 2024-08-05T11:39:49+08:00
draft: false
tags: ["Linux环境编程"]
categories: ["Linux环境编程"]
featuredImage: ""
featuredImagePreview: ""
---

## main函数参数

平时我们在学习C语言时，经常看到main函数的定义都没带参数，甚至也没有返回值，程序也能正常运行：

```c
void main();

```

这是因为我们学习中大多数都不需要main函数的参数和返回值，所以没有在乎这些细节。

程序在运行时，有时候需要给程序传递命令行参数，这时候就需要用到main函数的参数。

一个程序总是从main函数开始执行，main函数的原型为：

```c
int main(int argc, char* argv[]);
```

`argc`：命令行参数的数目。

`argv`：指向参数的各个指针所构成的数组。

示例：

```c
#include <stdio.h>

int main(int argc, char** argv)
{
    for (int i = 0; i < argc; ++i)
    {
        printf("参数[%d]:%s\n", i, argv[i]);
    }
    return 0;
}
```

运行结果：

```
$ ./main_test arg1 arg2 arg3
参数[0]:./main_test
参数[1]:arg1
参数[2]:arg2
参数[3]:arg3
```

从运行结果可以看到，第一个参数就是程序本身的名字，后面的参数依次是我们运行这个程序并附加的参数。然后我们就可以拿到这些参数，在程序中使用，这在shell脚本中很常见。

## main函数返回值

程序终止时，有时候我们需要返回值，比如shell脚本中，需要判断一个程序运行返回成功，才能继续向下执行，这时候就需要main函数返回值。

程序终止分为**正常终止**和**异常终止**：

正常终止：

1. 从`main`返回。
2. 调用`exit`。
3. 调用`_exit`或`_Exit`。
4. 最后一个线程从其启动例程返回。
5. 最后一个线程调用`pthread_exit`。

异常终止：

1. 调用`abrot`。
2. 接到一个信号。
3. 最后一个线程对取消请求做出响应。

当一个C程序被执行时，会使用一个exec函数，调用程序的main函数之前，先调用一个特殊的**启动例程**，可执行文件将这个启动例程指定为程序的起始地址，启动例程再从内核取得命令行参数为调用main函数做好准备。

当main返回后，会立即调用启动例程的exit函数，大概如下：

```c
exit(main(argc, argv));
```

可以手动调用exit函数终止进程：

```c
#include <stdlib.h>
void exit(int status);
```

exit函数都有一个参数，这个参数表示终止状态。大多数Linux系统的shell都提供检查进程终止状态的方法。可用 `echo $?` 查看进程的返回值。

main函数返回一个整形值和调用 exit(整形值) 是等价的，`exit(0) == return 0`。

如果：

1. 调用exit函数不带终止状态。
2. main执行了一个无返回值的return语句。
3. main没有声明返回类型为整形。

那么该进程的终止状态是未定义的。

如果main返回类型为整形，并且main执行到最后一条语句时返回（隐式返回），那么该进程的终止状态为0。

比如：

```c
#include <stdio.h>
main()
{
    printf("hello world!\n");
}
```

运行结果：

```
$ ./main_test 
hello world!
$ echo $?
0
```
