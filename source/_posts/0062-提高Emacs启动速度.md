---
title: 提高Emacs启动速度
date: 2024-06-18 19:12:49
tags:
my: speedup_emacs_startup_time
---
最主要的手段是Lazy load（延迟加载）。
分为入门版和高手版：
入门版：
Use-package中，给elisp插件设
:hook
:bind
:defer t
等关键字，插件就只会在特定情况下加载，从而提高启动速度。

高手版：
不用use-package，也不引入elpa的包
只当用到函数的时候去加载这个函数的文件和自己的设置。
这样，Emacs启动的时候就不用读取elpa文件中所有的包，降低启动速度。
比如[利用lazy-load.el按需加载Emacs插件](https://manateelazycat.github.io/2019/05/05/lazy-load/)

其它的方式:
包括：
提高垃圾回收阈值
在early-init.el文件中禁止加载一些frame。比如

```lisp
(tool-bar-mode -1); 关闭工具栏，tool-bar-mode 即为一个 Minor Mode
(setq inhibit-splash-screen t)               ;关闭首页
```

一定程度上也能提升启动速度。
