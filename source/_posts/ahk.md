---

title: AutoHotKey脚本的个人使用
date: 2021-10-29 14:14:14
tags: 
- 折腾
- 软件
categories: 
- 工具

---


最近觉得使用Windows的时候有些自动化的东西可以通过自动化或者是脚本来提高效率，于是去写了点简单的AutoHotKey脚本，其官方网站是：https://www.autohotkey.com
我的需求主要有几个方面：

1. 快捷键
2. 自动化
## 快捷键
Windows的快捷键有时候是蛮不规整，按起来也不方便。
比如关闭软件的快捷键是Alt+F4。
在笔记本上，按F4需要按Fn+F4两个键。再和Alt一起按，小拇指就十分的别扭。
之前用习惯i3wm的快捷键，于是把用AHK脚本把i3wm的快捷键：$mod+Shift+Q映射为了Alt+F4.
```
#+q::Send !{F4}
```
#表示Win，+表示Shift，q为q。
！表示Alt，{F4}表示为F4
以及i3wm还有一个$mod+Enter打开终端的快捷键，AHK脚本中可写为
```
#Enter::Run, PowerShell -Command "wt -p Arch"
```
`wt -p Arch`表示打开Arch的WSL子系统。wt的其他方法可见[官方文档](https://docs.microsoft.com/zh-cn/windows/terminal/command-line-arguments?tabs=windows)

## 自动化
我需求很简单，就是懒得一个一个打开链接和应用，所以直接
```
Run, xxxx.exe
Run, www.xxxx.com
```
这样运行脚本就可以一口气打开很多个网页和软件。省的自己一个一个去点击了。



