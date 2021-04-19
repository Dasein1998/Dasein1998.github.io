---

title: 安装Arch后常用命令
date: 2021-02-21 14:14:14
tags: 
- 折腾
categories: 
- 工具
---
[TOC]
本文主要记录一下自己安装Arch以及其衍生版本所需要完成的操作，以防每次都去谷歌。



### 安装Keyring

```bash
pacman-key --populate
```

### 修改源

源的位置为：`/etc/pacman.d/mirrorlist` 

中科大 Server = [https://mirrors.ustc.edu.cn/manjaro/stable/$repo/$arch](https://link.zhihu.com/?target=https%3A//mirrors.ustc.edu.cn/manjaro/stable/%24repo/%24arch)

清华大学 Server = [https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/manjaro/stable/%24repo/%24arch)

上海交通大学 Server = [https://mirrors.sjtug.sjtu.edu.cn/manjaro/stable/$repo/$arch](https://link.zhihu.com/?target=https%3A//mirrors.sjtug.sjtu.edu.cn/manjaro/stable/%24repo/%24arch)

浙江大学 Server = [https://mirrors.zju.edu.cn/manjaro/stable/$repo/$arch](https://link.zhihu.com/?target=https%3A//mirrors.zju.edu.cn/manjaro/stable/%24repo/%24arch)

​	安装AUR支持：

```
sudo pacman -Syu 
sudo pacman -S archlinuxcn-keyring
```



### SSH

```
ssh-keygen 生成密钥
```

密钥默认会保存在~/.ssh文件夹中。

连接GitHub的时候需要把私钥复制过去。

### 添加用户

官方文档：[Users and groups](https://wiki.archlinux.org/index.php/users_and_groups) 

```
useradd -m user 添加名为user的用户
passwd user 为user设置密码
sudo vim /etc/sudoers 使用vim修改user为root用户
```

### 安装yay

利用yay来帮助从AUR库中下载软件

```
sudo pacman -S git go base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

以后就可以通过yay 来进行软件下载和系统更新了（注意：yay 不能在root状态下使用）



### 安装zsh

```
sudo pacman -S zsh 安装zsh
sudo chsh -s /bin/zsh user 将zsh设置为user的默认shell
```

## 安装输入法

```
sudo pacman -S fcitx-im
sudo pacman -S fcitx-cofigtool 	fcitx-rime
```

然后

```
sudo vim .xpofile  然后在里面添加：
export XIM=fcitx
export XIM_PROGRAM=fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

