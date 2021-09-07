---

title: wsl2设置代理
date: 2021-09-03 14:14:14
tags: 
- 折腾
categories: 
- 工具
---

### wsl2设置代理：

>主机 IP 保存在 /etc/resolv.conf 中
>

最简单的方法：新建一个`.proxy`文件，内容为

```sh
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export all_proxy="socks5://${hostip}:7890"
```

`/etc/resolv.conf`中有主机的ip地址。

然后使用的时候:

```sh
source .proxy
```

---

稍微复杂点：在`.zshrc` 或者是	`.bashrc`中添加

```sh
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
alias setss='export https_proxy="http://${hostip}:7890";export http_proxy="http://${hostip}:7890";export all_proxy="socks5://${hostip}:7890";'
alias unsetss='unset all_proxy'
```

在终端输入**setss**即可使用代理，输入**unsetss**可取消代理

---

如果wsl连接不到主机，需要放开 `vEthernet (WSL)` 这张网卡的防火墙，以及让**Clash** 等代理软件能通过防火墙。

---

### 安装ohmyzsh 

```curl
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

打开`.zshrc`，找到`ZSH_THEME="robbyrussell"`，把‘’里的改成自己喜欢的图像，比如我喜欢的**lambda**

---

### 安装yay

我WSL2 用的是[别人打包好的Arch](https://github.com/yuk7/ArchWSL) 。默认的是pacman 的软件管理器，但是如果想从aur中下载软件的话，还是得用上yay。

```shell
sudo pacman -S git go base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

而这样运行会遇到`go: github.com/Jguer/aur@v1.0.0: Get "https://proxy.golang.org/github.com/%21jguer/aur/@v/v1.0.0.mod": dial tcp 142.251.43.17:443: i/o timeout`

的问题，网上搜索出来的解决方案是给Go 配置goproxy。

```go
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

而在终端输入这些后，goproxy是改过来了，但是`makepkg -si`依旧失败。
而输入：

```
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
```

则可以继续安装yay了。

