---

title: ubuntu 服务器使用
date: 20230128 09:22:17
tags: 
- 
categories: 
- 
my: update_ubuntu
---
阿里云的服务器到期了，买了台只有ipv6地址的服务器。
其提供的镜象都是19年的，只有ubuntu18，于是第一件事是升级ubuntu。
在root权限下输入如下代码：
```bash
apt update 
apt upgrade 
apt autoremove
apt install update-manage-core
do-release-upgrade
```
然后它会建议你不要在ssh的情况下更新，然后提醒你开一个新端口：1022。
一路选y，确认。
第一次升级到 ubuntu20。
再重复一次，升级到了ubuntu22。


## 创建新的用户
```shell
sudo adduser username 
```
输入两次密码
然后讲用户添加到sudo用户组，以便以后获取root权限。
```shell
sudo usermod -aG sudo username
```

## 安装 neovim：
ubuntu 默认的源，neovim 版本是 0.6，而截自 2023 年 1 月，neovim 的最新版本是 0.8。
所以需要添加源来安装最新的 neovim 
```bash
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt-get update
sudo apt-get install neovim
```
然后从github 下载neovim的配置
```bash
git clone https://github.com/dasein1998/nvim.git ~/.config/nvim
```
安装pip和npm，作为neovim里安装lsp的依赖
```
sudo apt install python3-pip
sudo apt install npm
```
然后安装依赖：
```
pip install pynvim
npm i -g neovim
```
进入nvim，输入`:Lazy`
则自动会安装好插件和lsp。

![](https://vip2.loli.io/2023/01/28/gjQefrcEtwUuZ5p.png)


## 安装zsh 和 ohmyzsh
```sh
sudo apt install zsh 
```
```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
如果提醒没有安装curl，则需要`sudo apt install curl`

## 安装emacs
```
sudo add-apt-repository universe
sudo apt update
sudo apt install emacs
```
或者安装最新版本
```
sudo apt-add-repository ppa:ubuntu-elisp/ppa
sudo apt update
sudo apt install emacs-snapshot

```
然后安装doom emacs
```sh
git clone --depth 1 https://github.com/doomemacs/doomemacs ~/.emacs.d
~/.emacs.d/bin/doom install
```



# Reference
[如何在Ubuntu创建和删除用户 | myfreax](https://www.myfreax.com/how-to-add-and-delete-users-on-ubuntu-18-04/)
[How to Install the Latest Emacs on Ubuntu](https://itsfoss.com/install-emacs-ubuntu/)
