---

title: 安装配置todo.txt-cli
date: 2021-02-24 12:12:12
tags: 
- 折腾
categories: 
- 工具

---
闲来无事，逛到一个项目：todo.txt 。顾名思义，是使用一个todo.txt文件来做todo list。

<div class="post-svg-container">
    <object type="image/svg+xml" data="/images/todo.txt.svg"></object>
</div>

 图片就是todo.txt完整的组成结构 ：

1. 头部的x 表示完成与否
2. (A)表示任务优先级
3. 2016-05-20表示任务完成时间
4. 2016-04-20表示任务创建时间
5. 后续内容描述任务
   1. 后面的 measure space for 表示 任务 +chapelShelving表示项目标签
   2. @表示内容标签

作者为这个开发了一个cli程序，[todo.txt-cli](https://github.com/todotxt/todo.txt-cli)，通过命令行管理todo.txt。我在安装的时候遇到了一些坑，在此记录一下。

（当前只是为了折腾而折腾，我也不是一个喜欢做GKD的人。本来是想玩玩org-mode的，但是org-mode入门实在是太复杂了，就先玩玩这个吧）

## Windows下配置

为使用PowerShell来使用todo.txt-cli，先在[Releases 界面](https://github.com/todotxt/todo.txt-cli/releases) 下载编译好的文件，

然后解压到自己喜欢的地方（比如我解压到`"D:\tools"`）

将解压后的文件位置添加到环境变量中（比如我解压出来在`"D:\tools\todo.txt_cli-2.12.0.dirty"`）
在**~**（$HOME）目录下，添加一个名为.todo的文件夹，里面添加一个config文件（注意：不带后缀），将下载的文件中名为`todo.cfg`的文件内容复制进去，作为用户自己的配置文件。



里面有很多可以自定义的地方：

### todo.txt文件位置

默认配置会将生成的`todo.txt` ,`done.txt` 保存在`todo.sh` 的文件夹中

```
export TODO_DIR=$(dirname "$0")   
# Your todo/done/report.txt locations
export TODO_FILE="$TODO_DIR/todo.txt"
export DONE_FILE="$TODO_DIR/done.txt"
export REPORT_FILE="$TODO_DIR/report.txt"
```

可以看出，todo.sh会将文件放入`TODO_DIR` 所定义的文件夹中，这个可以根据自己需求来自定义。

## 踩坑记录

这里有几个很坑的地方需要注意：

### 1. PowerShell不能直接使用.sh文件

恩。。如果是在unix的系统里，安装好后是可以直接运行todo.sh add “你要添加的todo”来添加todo的，但是Windows不能直接运行.sh文件，这里有两种选择：

1. 安装了Git：可以使用Git Bash来运行。在PowerShell中，可以`sh todo.sh` 来运行.sh文件

2. 安装了WSL（*Windows Subsystem for Linux* ）：可以直接 bash todo.sh

### 2. 自定义todo.txt的保存文件夹需要绝对路径

我的目的：把todo.txt和done.txt放在桌面，这样可以随时查看。

习惯性想法：去文件浏览器里复制桌面的路径（我的是`‪C:\Users\Name\Desktop\`），粘贴到`.todo/config`文件中的`TODO_DIR=`后面。运行`sh todo.sh add "写博客" ` ，桌面没有相应文件（`‪C:\Users\Name\Desktop\todo.txt`），而是在~（或者`$HOME`目录下），建了一个`C user name desktop`文件夹。这就说明我填写的路径不被正确识别。

思考：我既然用的是Git Bash 那么路径应该写桌面对应的，在bash的路径。于是在桌面上点击右键，选择Git Bash Here 可以看到显示的是`~/Desktop` 。将`~/Desktop` 粘贴到到`.todo/config`文件中的`TODO_DIR=`之后，然后再运行，发现桌面生成了todo.txt, done.txt 和 report.txt 。

### 3. WSL2-Arch 使用todo.txt-cli

Arch 不愧是包最多的发行版之一。AUR有人打包好了[todo.txt-cli](https://aur.archlinux.org/packages/todotxt/) 。可以直接

``` 
yay -S todotxt
```

恩 然后你会发现安装失败，终端显示：Cannot find the strip binary required for object file stripping

搜一搜，发现是没有安装make所需要的库：base-devel，于是

``` 
yay -S base-devel
yay -S todotxt
```

安装完成。同样的，要将`todo.cfg`文件复制到`~/.todo/config` 文件中：

（终端会有提示，直接cp 过去就行）

```shell
cp /usr/share/todotxt/todo.cfg ~/.todo/config
```

然后修改config文件，将保存目录改为桌面。
Windows的文件系统被WSL挂载在了mnt分区上，可以直接在桌面右键选择Linux Bash Here（我选的是Open in Windows Terminal）,可以看到当前的目录为：`/mnt/c/Users/Name/`，在后面添加Desktop/就行了。于是修改为`/mnt/c/Users/Dasein/Desktop/` 。这样就可以在PowerShell和WSL中都使用todo.txt-cli了，共用同一个任务体系。

## 使用方式

官方的使用介绍：[USAGE.md](https://github.com/todotxt/todo.txt-cli/blob/master/USAGE.md)



## 个人使用体验

待续~~~







