---

title: 使用Obsdian的Emacs快捷键
date: 20230801 19:45:12
tags: 
- 折腾 
categories: 
- 
---
上次写到使用obsdian提供的vim快捷键模式。vim在中文输入时总是会面临中文输入法无法在vim的normal模式顺畅使用快捷键的问题。通过自动化切换输入法可以缓解这个问题。
但在日常使用中，还是Emacs的快捷键比较顺手，obsidian可以通过一系列的插件配置来实现emacs快捷键的复刻。
首先，修改ob自带的快捷键，比如将命令面板的快捷键改成Alt+X。
![](https://vip2.loli.io/2023/08/01/xmM5T19NaAR8dFy.png)
比如

安装插件Editor Commands Remap，可以将配置诸如：光标上移一行C-p、光标下移一行C-n、光标向前移动一个词M-f、光标向后移动一个词M-b。其他的可以根据自己对Emacs快捷键的需求进行配置。
![](https://vip2.loli.io/2023/08/01/7w3iQID6kGnKFsX.png)
配合上述插件来进行单词跳转的另一个插件为：Word Splitting for Simplified Chinese in Edit Mode and Vim
Mode 。插件可以进行中文的分词，可以在中文内容中进行词语的跳转

安装插件Hotkeys++还可以获得更多功能的快捷键，比如清除当前行等。
![](https://vip2.loli.io/2023/08/01/iFDWjQxL7MUBPNm.png)

# Reference
[Emacs常用快捷键一览 - Yuan Qiu](https://qiutedyuan.github.io/blog/2019/06/12/Emacs%E5%B8%B8%E7%94%A8%E5%BF%AB%E6%8D%B7%E9%94%AE%E4%B8%80%E8%A7%88/)


