---
title: "Guide To Syscalls"
date: 2017-08-03T22:13:21+08:00
draft: false
---

{{< youtube 01w7viEZzXQ >}}

#   笔记

-   什么是syscalls
    -   用户程序调用kernel services
    -   文件，设备，进程，通信，时间
    -   strace可以看到
-   syscall如何工作
    -   strace -c 统计syscall
    -   Syscall是一个兼容层，用来兼容各种设备
    -   syscall是设置寄存器和变量，kernel执行，取回结果
    -   有许多是不常用的：据说Bash on Windows只实现了一部分
-   玩一下ptrace
    -   strace通过ptrace来获取syscall的调用信息
    -   自己实现一个strace玩一玩
    -   通过设置ptrace的SysProcAttr，实现自己的strace
    -   PTRACE_SYACALL有两次停顿，调用时和结束时，并且tracer是区分不出来的
-   Syscall与安全
    -   限制程序只执行syscall一部分子集
    -   在微服务的架构下很有实际意义
    -   docker可以通过seccomp文件，限制syscall的子集

#   相关链接

-   https://github.com/lizrice/strace-from-scratch