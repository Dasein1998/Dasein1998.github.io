---

title: 使用Obsdian的Vim快捷键
date: 20221108 19:45:12
tags: 
- 折腾 
categories: 
- 
my: Vim_in_Obsidian

---
obsdian提供了vim的快捷键模式。
但vim在中文输入时总是会面临中文输入法无法在vim的normal模式顺畅使用快捷键的问题。
然后发现ob的插件商店里有人利用im-select提供了一种自动化切换输入法的解决方案。
[ALONELUR/vim-im-select-obsidian: Obsidian plugin: vim im select](https://github.com/ALONELUR/vim-im-select-obsidian)
安装方式就是：在ob的插件中搜索Vim IM select。
配置方式，官方文档语焉不详，于是我简要介绍一下。
## 安装im-select
首先，下载 [im-select](https://github.com/daipeihust/im-select)，可以用scoop安装，也可以自己手动安装。
```powershell
scoop bucket add im-select https://github.com/daipeihust/im-select
scoop install im-select
```
手动安装的话，就随便放在一个没有中文的文件夹中或者添加到windows的系统变量中。

## 配置英文输入
![](https://vip2.loli.io/2022/11/08/5JbivWEYkuxaHcl.png)
打开设置中的语言，添加语言-添加英语-
这时候就添加好了系统的英文输入法。
## 配置 Vim IM select
然后打开Vim IM select的插件配置界面，可以看到，对于Windows，它有三个配置位置。
`Default IM` 
`Obtaining Command`
`Switching Command`
这时候，打开 powershell，输入 im-select.exe，它会出现一段数字，表示当前输入法的数值。然后填入 `Default IM` 中。比如我的中文输入法是2052，英文是1033。这里一般填写中文输入法的数字，
`Obtaining Command` 填写 im-select.exe 的路径，如 我使用scoop安装，填写的就是`C:\Users\user\scoop\shims\im-select.exe`
`Switching Command` 在 im-select.exe 的路径后加入 locale 参数。它就会切换输入法到另一种语言。填写的内容是：`C:\Users\user\scoop\shims\im-select.exe locale`
这时候回到ob的编辑，就可以体验到：Esc退出编辑模式，输入法自动切换为英文，然后按i 或者a o 进入编辑模式后，会自动变回中文输入法。
这样写中文笔记就很愉快了



# Reference
[vim-im-select-obsidian/README_zh.md at master · ALONELUR/vim-im-select-obsidian](https://github.com/ALONELUR/vim-im-select-obsidian/blob/master/README_zh.md)

# FAQ
设置系统变量的方式：我的电脑【右击】 --> 选择 属性 --> 高级系统设置 --> 环境变量
[windows10 环境变量设置_palmer_kyle的博客-CSDN博客_环境变量](https://blog.csdn.net/palmer_kai/article/details/80588594)
[windows系统如何设置添加环境变量-百度经验](https://jingyan.baidu.com/article/47a29f24610740c0142399ea.html)

设置英文输入法
[Windows10添加英文输入法_烟火笑风尘的博客-CSDN博客_win10添加英文输入法](https://blog.csdn.net/moshiyaofei/article/details/102852186)


