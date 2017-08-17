---
title: "Go Build Modes"
date: 2017-08-16
tags: ["Go", "talk"]
draft: true
isCJKLanguage: true
---

{{< youtube x-LhC-J2Vbk >}}

总共8中build类型，是-buildmode的选项

### exe(static)

self-contained，包含所有的调用，没有外部依赖，是大多是情况下go语言编译的输出

### exe(with libc)

会依赖系统的libc，在涉及网络如DNS查找，或者用户态的系统调用，如操作系统用户查找的时候，会自动加上。

### exe(with more)

go语言调用其他的语言的场景，符合ELF ABI

### pie

PIE: Position-independent executables

-build_mode=pie

在生成字节码的时候，一般是让成名jump到指定的地址，这样恶意代码可以让你在jump之后执行它自己的代码。

而PIE是相对地址，所以让恶意程序更难去注入自己的代码

在安卓是默认的，需要操作系统层面的支持，是未来的趋势

### c-archive

生成.a文件，C可以静态链接

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

生成.so(shared object)

可以通过`LD_LIBRARY_PATH`加入.so查找目录

```shell
$ go build -buildmode=c-shared hello.go

$ cc hello.a hello.so

$ LD_LIBRARY_PATH=. ./a.out
Hello, World!
```

Android调用go是这样实现的

### shared

像C一样，用go程序打包成so的代码，然后由go来调用

### plugin

实际的例子，