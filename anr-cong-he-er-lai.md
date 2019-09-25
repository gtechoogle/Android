---
description: 为什么会有 ANR，是什么造成了 ANR 的发生
---

# ANR 因果关系

Application Not Response 简称 ANR，从字面的意思也不难理解，就是应用无响应，但是具体来说，什么样的情况叫做应用无响应呢? 无响应的定义是什么呢？ 和通常理解的手机 hang 住，或者 SWT （Software watchdog timeout） 的差别是什么呢? 

想要回答上面的问题，就又要回归到 Android 自身，其实每个机制或者技术的产生，都是有一定的历史原因，清楚了背景才能更加理解其设计的初衷。

下面的图是一个基本的 Activity UI thread 的工作示意图

