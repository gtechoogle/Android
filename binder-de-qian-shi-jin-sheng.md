---
description: 彻彻底底理解 Binder 的工作原理
---

# Binder 前世今生

什么是 Binder? 为什么在 Android 上才会出现 Binder 呢? 那么没有 Binder 的时候是怎么工作呢? 带你进入 Android 的 Binder 世界

### Binder 的诞生

Binder 是为了跨进程通讯，这里就带来了一个基本问题，什么是进程，什么叫做跨进程?

#### 进程

进程就是**执行中**的程序，是资源分配的最小单位

当程序运行的时候，操作系统会为这个程序提供它需要的资源。例如 memory for storing data, system resource like file system access. 一个进程其实就是一个 container, 其中存放了运行需要的资源，和这段运行的代码。

所以两个进程就是两个独立的资源，因此带来的问题就是无法互相访问，比如进程 A 是无法去直接访问 进程 B 的变量，因此这里就引申出来跨进程通讯

#### Linux 跨进程通讯方式





