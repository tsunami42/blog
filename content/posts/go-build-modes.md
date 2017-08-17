---
title: "Go Build Modes"
date: 2017-08-17
tags: ["Go", "talk"]
isCJKLanguage: true
---

{{< youtube x-LhC-J2Vbk >}}

总共8中build类型，是-buildmode的选项

## exe (static)

self-contained，包含所有的调用，没有外部依赖，是大多是情况下go语言编译的输出。

## exe (with libc)

会依赖系统的libc，在涉及网络如DNS查找，或者用户态的系统调用，如操作系统用户查找的时候，会自动加上。

## exe (with more)

go语言调用其他的语言的场景，符合ELF ABI

适合有legacy系统的场景，构建很复杂。

## pie

PIE: Position-independent executables

-build_mode=pie

在生成字节码的时候，一般是让成名jump到指定的地址，这样恶意代码可以让你在jump之后执行它自己的代码。

而PIE是相对地址，所以让恶意程序更难去注入自己的代码

在安卓是默认的，需要操作系统层面的支持，是未来的趋势

不过现在生成的binary偏大，并且会有差不多1%的性能损耗

## c-archive

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

**不过我按照上面的代码来build的时候，提示缺少hello.h。**

iOS程序调用go是通过这种方式实现的

## c-shared

生成.so(shared object)

可以通过`LD_LIBRARY_PATH`加入.so查找目录

```shell
$ go build -buildmode=c-shared hello.go

$ cc hello.a hello.so

$ LD_LIBRARY_PATH=. ./a.out
Hello, World!
```

Android调用go是这样实现的

## shared

用go程序打包成so，然后由go来调用动态

```shell
$ go install -buildmode=shared std

$ go build -linkshared hi.go

$ ./hi
Hello, World!
```

我本地运行的结果，的大小是15K。
```shell
➜  shared git:(master) ✗ ls -l hello
-rwxr-xr-x 1 root root 15182 Aug 17 09:53 hello
➜  shared git:(master) ✗ ldd hello  
	linux-vdso.so.1 =>  (0x00007fff536b6000)
	libstd.so => /root/src/go/pkg/linux_amd64_dynlink/libstd.so (0x00007fc493c16000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fc493874000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007fc493670000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fc493453000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fc49604b000)
```
节省硬盘空间（好吧），节省RAM，公共的代码只会被装载一次，部署复杂

## plugin

```go
// myplugin.go
package myplugin

import "fmt"

func Hello() {
	fmt.Println("Plugin: Hello, World!")
}
```

```go
// runplugin.go

package main

import (
	"log"
	"plugin"
)

func main() {
	p, err := plugin.Open("myplugin")
	if err != nil {
		log.Fatal(err)
	}
	fn, _ := p.Lookup("Hello")
	if err != nil {
		log.Fatal(err)
	}
	fn.(func())()
}
```

```shell
$ go build -buildmode=plugin myplugin

$ go build runplugin.go

$ ./runplugin
Plugin: Hello, World!
```

构建和部署都很复杂，不顾还有有使用的场景，在最后花了差不过7分钟来介绍这个真实的使用场景。

**不过demo我也没有跑通┑(￣Д ￣)┍**