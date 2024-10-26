---
title: 在Emacs中使用中Denote做笔记
date: 2024-10-20 12:12:12
tags: 
- 折腾 
categories: 
my: Denote_in_Emacs
---
[Denote \(denote.el\) | Protesilaos Stavrou](https://protesilaos.com/emacs/denote)是一个以命名为核心，基于Org mode的Emacs笔记插件。作者是 Protesilaos ，一位哲学家和Emacs用户。
他在[自己的博客](https://protesilaos.com/about/)里说到：

```
In a manner of speaking, I just talk to programmers about philosophy and to philosophers about programming.
```

Denote 的核心理念在于它的命名思路，Protesilaos 认为文件名需要清楚表达文件内容，而不需要额外数据参考。这不仅适用于笔记，也适用于所有的视频、图片等文件。Denote默认对笔记的命名为：日期和时间--标题__关键词.文件格式（DATE--TITLE__KEYWORDS.EXT）。
其中日期为“年月日T时分秒”的一串数字和字母（YYYYMMDDTHHmmss），比如这个笔记的创建时间为20241020T121212.
在Denote中，每一个文件的区分以创建时间为主，不同的时间表示不同的文件，每一个文件以它的创建时间不变量。
这点和Denote的核心功能：“引用”有关，假如有一个文件为20241020T121212--Denote_in_Emacs__Emacs.md。在其它笔记引用这个文件时，Denote是用20241020T121212来引用这个文件的。
这样有一个优点：用户可以用任何软件随意修改文件的名称和关键词，而无需在修改后去其它文件中同步修改引用时的文件名。
缺点就是：不能在同一秒创建多个文件，Denote无法区分同一秒的其它文件。
Denote的基本配置如下：

```lisp
(use-package denote
  :ensure t
  :hook (dired-mode . denote-dired-mode)
  :bind
  (("C-c n n" . denote)
   ("C-c n r" . denote-rename-file)
   ("C-c n l" . denote-link-or-create)
   ("C-c n b" . denote-backlinks)
   ("C-c n k" . denote-rename-file-keywords)
   ("C-c n s" . denote-silo-extras-open-or-create)
   ("C-c n o" . denote-open-or-create)
   )
  :config
  (setq denote-directory (expand-file-name "~/note"));;主笔记库的位置
  (setq denote-rename-confirmations nil) ;;修改名字时免确认
  ;;(setq denote-file-type 'org)
  (denote-rename-buffer-mode 1)
  (require 'denote-silo-extras) ;;添加对次笔记库的支持
  (setq denote-silo-extras-directories   ;;其它笔记库的路径
 `("~/note1/"
  "~/note2/"))
  )
```

由于，Denote 使用了xargs作为反向链接的搜索工具，而Windows不自带xargs工具。
但Git for windows中自带xargs，于是把git的库填写到emacs的Path中，是一种可行的方法。

```lisp
 (when (eq system-type 'windows-nt) ;; windows specific settings
    (setenv "PATH" (concat (getenv "PATH") ";" "C:\\Program Files\\Git\\usr\\bin")));;git的默认安装路径
```


