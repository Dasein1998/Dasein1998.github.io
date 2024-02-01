---

title: Git基本使用
date: 2021-09-13 14:14:14
tags: 
- 折腾
categories: 
- 工具
---
每次换系统就需要重新安装、配置Git，于是记录一下配置的步骤。

GitHub官方文档：[中文版](https://docs.github.com/cn/github/getting-started-with-github/using-git)

SSH文档：[阮一峰的SSH教程](http://wangdoc.com/ssh)

1. 下载Git：[Git下载](https://git-scm.com/downloads) 

   1. Windows和Mac直接下载安装包就行了。

   2. Arch的话 直接`sudo pacman -S git`  

2. 然后配置用户名和邮箱：

   ```
   git config --global user.name "your name"
   ```

   ```
   git config --global user.email "your email"
   ```

   也可以直接编辑`.gitconfig`文件，默认是在用户的文件夹下。

   在里面加入：

   ```
   [user]
   	name = user's name
   	email = user's email
 
   ```
   有需要加代理的，可以加入
   ```bash
   [https]
   	proxy = socks5://127.0.0.1:7890
   [http]
   	proxy = socks5://127.0.0.1:7890
   ```

   Windows在"C:\Users\\"里，Linux在`~/.gitconfig`

 3. 设置SSH

    命令行输入`ssh-keygen `，默认会在~/.ssh文件夹中生成`id_rsa` 和`id_rsa.pub`文件，`id_rsa`是私钥，而`id_rsa.pub`是公钥。

    把公钥的内容复制到GitHub中，然后

    ```
    ssh -T git@github.com
    ```

    检查是否能够SSH连接到GitHub上，应该会出现的是:

    ```
    > Hi username! You've successfully authenticated, but GitHub does not
    > provide shell access.
    ```

    

4. 日常使用

   我一般就用HEXO写博客和使用Logseq记笔记的时候使用Git，对Git的项目管理等功能使用教少，其他操作可以参考：[Git中文文档](https://git-scm.com/book/zh/v2)

   ```
   git clone   	 #下载GitHub上的仓库
   git add . #将文件夹中其他新项目添加进Git中
   git commit -m '评论的内容'
   git status #查看Git状态
   git push #推送到GitHub上
   ---
   git init #将文件夹初始化为Git仓库
   ```

   ---

5. ​	多Git平台配置

   在~/.ssh/文件夹中新建config文件，其内容写为：

   ```
   # gitee
   Host gitee.com
   HostName gitee.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/gitee_id_rsa
   # github
   Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/github_id_rsa
   ```

   其中`Host`为Git的服务器地址，`IdentityFile`为私钥，为了方便起见，不同平台的私钥尽量取不同的名字。

6. Arch 使用

   Arch使用Git前需要安装[Openssh](https://wiki.archlinux.org/title/OpenSSH#Installation)

   ```
   sudo pacman -S openssh
   ```

   7.简单的自动化脚本：

   写了一个简单的bat脚本，用于windows下将logseq的文件夹同步到GitHub上

   ```bash
   @echo off
   @title git push
   cd D:/notebook
   D:
   git add .
   git commit -m '%DATE%'
   git push
   ```

   FAQ:

   1. 出现fatal: No configured push destination.

      ```bash
      git add .
      git commit -m %DATE%
      git push
      ```

      解决方案：

      ```bash
      git remote add origin https://github.com/xxxxx/xxxxx.git 
      git push -u origin master
      ```

      