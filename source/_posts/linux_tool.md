---

title: 提高Linux命令行体验的工具
date: 2021-10-22 14:14:14
tags: 
- 折腾
categories: 
- 工具
---
## [zsh](https://www.zsh.org)

zsh相比bash来说，功能更加强大。通过安装插件可以获得更便利的使用体验。

安装方式：

```bash
sudo pacman -S zsh $安装zsh
sudo chsh -s /bin/zsh user $将zsh设置为user的默认shell
```

可选：安装[oh-my-zsh](http://ohmyz.sh)

```sh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

有的人会觉得oh-my-zsh启动太慢，过于臃肿，可以卸载:

```bash
uninstall_oh_my_zsh
```

也可以通过zsh的插件管理来安装oh-my-zsh。如[zinit](https://github.com/zdharma/zinit)，提供了使用oh-my-zsh插件的方式：[Example Oh My Zsh Setup](https://zdharma.github.io/zinit/wiki/Example-Oh-My-Zsh-setup/)。
也可自选安装插件。

## [Z.sh](https://github.com/rupa/z)

一个非常经典的脚本，用于快速跳转到常用的位置。脚本会记录日常使用 cd 的位置，然后下次不需要“cd +全路径”，只需要“z+路径的关键字母”就行。

比如我博客的git仓库在：D:\blog，我每次从powershell进就需要`cd D:\blog`。
使用脚本后，只需要 `z b` 就行。（windows下，我使用的是[z.lua](https://github.com/skywind3000/z.lua)  ,配置方式可以看我的另一篇博客：[powershell折腾记录](https://dasein.site/2021/02/23/powershell/)）

Linux下安装，安装官方的方法：

```bash
Put something like this in your $HOME/.bashrc or $HOME/.zshrc:
              . /path/to/z.sh
```

 即，下载z.sh 然后在bash或者zsh的配置文件中添加上z.sh的路径即可。

Arch下，可以直接 `yay -S z` 安装。然后将

```sh
[[ -r "/usr/share/z/z.sh" ]] && source /usr/share/z/z.sh
```

添加到`.zshrc`或者`.bashrc`中。

也可以用sed工具：`1i`表示添加在第一行，也可以用`$a`来添加到最后一行。

```bash
sed '1i [[ -r "/usr/share/z/z.sh" ]] && source /usr/share/z/z.sh' .zshrc
```

## [tldr](https://github.com/tldr-pages/tldr)

我们常用man来查看某工具的文档，但是往往会因为文档过多而不知重点。tldr对man进行优化，提供工具更常用的文档。

官方解释：

> TL;DR stands for "Too Long; Didn't Read". It originates in Internet slang, where it is used to indicate that a long text (or parts of it) has been skipped as too lengthy. Read more in How-To Geek's article.

安装方式：

```sh
npm install -g tldr
```

或者

```sh
pip3 install -g tldr
```

使用方式和`man`相同，`tldr sed`查看sed的使用方式。
