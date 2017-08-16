---
title: "Go Build Modes"
date: 2017-08-16
tags: ["Go", "talk"]
draft: true
isCJKLanguage: true
---

{{< youtube x-LhC-J2Vbk >}}

总共8中build类型

### exe(static)

self contiains

### exe(with libc)

libc， DNS，user查找

在有网络的时候，会自动加上

用户空间

### exe(with more)

调用其他的语言

### pie

-build_mode=pie

一般是调到指定的地址，这样更容易被攻击

而PIE是相对地址

安卓是默认的



### c-archive

生成.a文件，C可以用

```go
package main

import "fmt"
import "C"

func main() {}

// export Hello
func Hello() {
    fmt.Println("Hello, World!")
}
```

```c
#include "hello.h"

int main(void) {
    Hello();
    return 0;
}
```

```shell
$ go build -buildmode=c-archive hello.go

$ cc hello.a hello.c

$ ./a.out
Hello, World!
```

iOS程序调用go是通过这种方式实现的

### c-shared

生成.so文件

加入.so查找目录

android  

### shared

像C一样，用go程序打包成so的代码，然后由go来调用

### plugin

实际的例子，