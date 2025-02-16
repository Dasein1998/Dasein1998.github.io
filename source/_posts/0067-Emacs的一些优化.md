---
title: 在Win10上使用Emacs的一些优化
date: 2024-10-26 11:12:13
tags: 
- 折腾 
categories: 
my: Emacs_on_Windows
---

Emacs虽然是一个全平台软件，但在Windows上使用总有一些问题。
首先，对Emacs使用影响最大的就是Windows的编码问题。
众所周知，Windows为了和老版本兼容，有很多历史包袱，尤其是对于cjk（中日韩）的编码。
对中文用户来说，Windows系统默认字符编码为gbk编码。
但对于编程语言来说，默认编码基本都是UTF-8。
Win10虽然系统能在 Administrative language settings 里勾选 Beta: Use Unicode UTF-8 for worldwide language来改成UTF-8，但这样很多老软件的中文字体会出现编码的问题而显示乱码。（听说部分（老）软件乱码可用 Locale Emulator 解决）
为了Emacs等少数应用来改系统编码有点得不偿失。
于是，只好通过改Emacs的配置文件来解决这个问题

```lisp
(defconst IS-WINDOWS (eq system-type 'windows-nt));;检查当前系统是不是Windows
(when (eq system-type 'windows-nt)
  (setq file-name-coding-system 'gbk))  ;;读文件名用gbk
```
原生Windows程序不是用unicode编码，而是用系统的locale编码，比如ripgrep在中文locale下面返回的数据是gbk编码。
而在Emacs中，Consult 中使用rg和everything-cli来检索文件，也需要改rg和Everything-cli(简称为es)的编码：

```lisp
(if sys/win32p
    (progn
    (add-to-list 'process-coding-system-alist '("rg" utf-8 . gbk));;解决counslt-rg无法搜索中文的问题，开启默认utf-8后就不需要了。
    (add-to-list 'process-coding-system-alist '("es" gbk . gbk))
    (add-to-list 'process-coding-system-alist '("explorer" gbk . gbk))
    (setq consult-locate-args (encode-coding-string "es.exe -i -p -r" 'gbk))))
```
这样就可以通过`consult-locate`命令在Windows上搜索本地文件了。
（前提是安装好了everything-cli，用scoop安装： scoop install everything-cli ,或去everything官网下载es.exe并添加到系统变量）

## Org-mode中，通过Powershell读取系统剪贴板并插入图片到特定文件夹中

```lisp
;;;从windows剪贴板插入图片
(defun my/org-insert-image-from-clipboard ()
  "Insert an image from the clipboard into the current org buffer."
  (interactive)
  (let* ((current-dir (file-name-directory buffer-file-name))
	  (file-name-base (file-name-base buffer-file-name))
	 ;;(attach-dir (concat current-dir "assets/" file-name-base "/"))
	 (attach-dir "assets/")
   (attach-dir-pic "./assets/")
	 (image-file (concat attach-dir (format-time-string "%Y%m%d%H%M%S") ".png"))
   (image-file-pic (concat attach-dir-pic (format-time-string "%Y%m%d%H%M%S") ".png")));;相对路径
    ;; Ensure attach directory exists
    (unless (file-exists-p attach-dir)
      (make-directory attach-dir t))
    ;; Save the clipboard image to the attach directory
    (if (eq system-type 'windows-nt)
	(shell-command (concat "powershell -command \"Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.Clipboard]::GetImage().Save('" image-file "', [System.Drawing.Imaging.ImageFormat]::Png)\""))
      (error "Unsupported OS")
      )
    ;; Insert the link to the image in the org file
    (insert (concat "[[file:" image-file-pic "]]"));;相对路径
    ;;(org-display-inline-images)
    )
  )
```

## Reference
[2023年windows下的正确编码设置 - Emacs-general - Emacs China](https://emacs-china.org/t/2023-windows/24920)
[部分（老）软件乱码可用 Locale Emulator 解决](https://www.v2ex.com/t/936616)
