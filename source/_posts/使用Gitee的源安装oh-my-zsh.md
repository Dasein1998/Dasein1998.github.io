---
title: 从Gitee安装oh-my-zsh
date: 2021-11-01 14:14:14
tags: 
- 折腾
- 软件
categories: 
- 工具
---
默认安装oh-my-zsh的方式是：

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

但是GitHub访问太慢，而Gitee有oh-my-zsh的镜象。但是Gitee是直接吧GitHub的包克隆下来了，README.md文件一字未动，安装方式也没有改成国内能用的方式。于是我照着官方的安装方式，使用Gitee的源安装了oh-my-zsh。记录下来，方便其它人参考。

1. 从Gitee下载oh-my-zsh

```bash
git clone https://gitee.com/mirrors/oh-my-zsh.git ~/.oh-my-zsh
```

2. 备份自己的.zshrc，重命名为.zshrc.orig或者自己喜欢的名字。

```bash
cp ~/.zshrc ~/.zshrc.orig
```

3. 创建一个新的zsh配置文件：使用oh-my-zsh的模板文件作为zsh配置文件。
```bash
   cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```
4. 切换默认终端为zsh
```bash
   chsh -s $(which zsh)
```
5. 重启zsh，然后可以自己改`.zshrc`来修改zsh的配置。

而据我猜测，官方更新是通过/tools/upgrade.sh来进行更新的，其中第129-138行出现如下字串：

```sh
git remote -v | while read remote url extra; do
  case "$url" in
  https://github.com/robbyrussell/oh-my-zsh(|.git))
    git remote set-url "$remote" "https://github.com/ohmyzsh/ohmyzsh.git"
    break ;;
  git@github.com:robbyrussell/oh-my-zsh(|.git))
    git remote set-url "$remote" "git@github.com:ohmyzsh/ohmyzsh.git"
    break ;;
  esac
done
```

我猜测把其中的`https://github.com/robbyrussell/oh-my-zsh`换成

```
https://gitee.com/mirrors/oh-my-zsh
```

就可以通过gitee进行更新了。暂未尝试。
