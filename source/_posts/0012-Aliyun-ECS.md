---

title: 阿里云学生机折腾记录
date: 2020-08-31 18:18:18
tags: 
my: Aliyun
---

过年后买了台阿里云的学生机，1核2g 1M带宽。

已搭建：

1. TinyTinyRSS
2. Frp

未搭建（未完成）：

1. Terraria私服
2. OLAINDEX (缺个域名)

在本文简要介绍一下以上内容。

# 已完成

## 一.TinyTinyRSS

**RSS**（全称：[RDF](https://zh.wikipedia.org/wiki/Resource_Description_Framework) Site Summary；Really Simple Syndication[[2\]](https://zh.wikipedia.org/wiki/RSS#cite_note-powers-2003-1-2)），中文译作**简易信息聚合**[[3\]](https://zh.wikipedia.org/wiki/RSS#cite_note-3)，也称**聚合内容**[[4\]](https://zh.wikipedia.org/wiki/RSS#cite_note-张锐2015-4)，是一种[消息来源](https://zh.wikipedia.org/wiki/消息來源)格式规范，用以**聚合经常发布更新资料的网站**，例如[博客](https://zh.wikipedia.org/wiki/部落格)文章、新闻、[音频](https://zh.wikipedia.org/wiki/音訊)或[视频](https://zh.wikipedia.org/wiki/視訊)的网摘。RSS文件（或称做摘要、网络摘要、或频更新，提供到频道）包含全文或是节录的文字，再加上发布者所订阅之网摘资料和授权的元数据。简单来说 RSS 能够让用户订阅个人网站个人博客，当订阅的网站有新文章是能够获得通知                                      ---摘自RSS的WiKi 词条

RSS的火爆已经是21世纪初期的事情了，在Google Reader于2013年停止服务宣告RSS退出历史舞台。在新闻媒体，微博，今日头条等聚合类资讯应用占领绝大多数市场的时代，只有极少的用户会继续使用RSS。现在很多平台的内容也不支持RSS订阅，比如知乎，微信公众号，B站等大多数现代媒体平台，支持RSS大多是个人或小团体的资讯平台，如个人博客，爱范儿，煎蛋等。（曾经有一段时间 有第三方平台提供微信公众号文章的RSS订阅，后来被微信封杀了，比如16，17年的微广场）现在有人做了个开源的RSS生成器 [RSSHub](https://docs.rsshub.app) ， 里面聚合了上百家网站的RSS订阅方式，包括微信公众号，知乎文章专栏等。

与诸如Inoreader等RSS服务不同，TinyTinyRSS是在自己服务器搭建的RSS服务，功能足够，配合RSSHub使用可以订阅很多个性化内容，ttrss可以在浏览器中或者适配的移动端软件中使用，我电脑端直接使用网页版，手机用的FeedMe。浏览器还可以安装RSSHub的拓展插件，通过插件连接自己的TinyTinyRss服务，在浏览可以RSS订阅的网站时可以一键通过RSSHub和TinyTinyRss的联动来将网站添加到订阅源中。

## 二.Frp

我有一个阿里云的学生机，也就有了一个公网的IP。于是可以利用我的公网IP实现内网穿透。总所周至，ipv4地址在几年前就已经用完了，很多人的家里都没有公网IP，都是运营商的大局域网分配的IP地址。而在局域网内像要访问其他局域网，比如在家要访问学校的设备，就无法直接连接，此时需要一个公网的服务器做节点，两台或多台处在自己的局域网的设备可以通过与公网服务器连接来达成相互连接。通过这个方法可以随时随地的远程控制处在不同局域网的设备。

Frp就是达成这个链接方式的工具。 在服务器客户端分别安装[Frp](https://github.com/fatedier/frp)，编辑ini文件，自定义端口和验证方式并让其后台运行。就完成了服务器和客户端的链接。此时可以通过访问公网地址和相应端口来访问局域网的设备。通过Remote Desktop（RDP协议）可以远程控制windows系统，通过ssh控制linux系统，可以通过HTTP(S)控制家里的路由器。

## 三.Gitea

Gitea是用go写的自建开源Git服务软件。可以较为容易的在自己的服务器上搭建，可以使用sqlite3作为数据库。

主要思路：

1. 建立名为git的用户，并授于权限

```
sudo adduser --system --group --disabled-password --shell /bin/bash --home /home/git --gecos 'Git Version Control' git
```

2. 创建文件

3. ```

   sudo mkdir -p /var/lib/gitea/{custom,data,log}
   sudo chown -R git:git /var/lib/gitea/
   sudo chmod -R 750 /var/lib/gitea/
   sudo mkdir /etc/gitea
   sudo chown root:git /etc/gitea
   sudo chmod 770 /etc/gitea

   ```

4. 下载gitea 可以去https://dl.gitea.io 中查看最新的版本

5. ```
   wget -O gitea https://dl.gitea.io/gitea/1.13.6/gitea-1.13.6-linux-amd64
   ```

6. 将程序移到  /usr/local/bin

7. ```
   mv gitea /usr/local/bin
   ```

8. 下载写好的systemd代码并放入/etc/systemd/system/文件夹

9. ```
   sudo wget https://raw.githubusercontent.com/go-gitea/gitea/master/contrib/systemd/gitea.service -P /etc/systemd/system/
   ```

10. 启动gitea的systemd服务

11. ```
    sudo systemctl daemon-reload
    sudo systemctl enable --now gitea
    ```

12. 默认会在3000端口开启网页控制面板。可以通过 服务器地址:3000 访问

ps：配置文件位置/etc/gitea/app.ini

# 待完成

### 1. Terraria私服

《**泰拉瑞亚**》（英语：**Terraria**）是一款2D[沙盒模拟游戏](https://zh.wikipedia.org/wiki/沙盒类游戏)，由Re-Logic开发。游戏2011年5月16日最初发布在[Windows](https://zh.wikipedia.org/wiki/Microsoft_Windows)平台，此后，陆续发布了支持其他操作系统、主机、智能手机和平板电脑等的版本。泰拉瑞亚的游戏特色是在一个[随机生成](https://zh.wikipedia.org/wiki/过程生成)的[2D](https://zh.wikipedia.org/wiki/2D)世界里探索、创造、建筑，并与各种生物战斗。游戏发布时受到普遍积极的评价，其沙盒元素颇受好评。截止到2020年4月累积销量逾3000万。                        ---摘自Terraria的WiKi 词条

[Terraria](https://terraria-zh.gamepedia.com/%E6%9C%8D%E5%8A%A1%E5%99%A8#.E5.BC.80.E6.9C.8D.E6.96.B9.E6.B3.95_.28Linux.29) 官方wiki提供了自建服务器的方法和[安装包](https://terraria.org/system/dedicated_servers/archives/000/000/039/original/terraria-server-1405.zip?159130136) 。看网上说法是搭建需要1.5g左右内存，支持两-三人玩耍。因为暂时没有玩这个的兴趣，于是先搁置了。

### 2. OLAINDEX

OLAINDEX是挂载Onedrive在网页上的工具，可以利用ondrive的教育版和商业版的1T的空间当作一个下载站。

但由于我没有备案的域名不能进行TLS加密，于是卡在了最后一步。等哪天有闲情再折腾。

## 3. 建站

本博客是基于HEXO框架和GitHub Pages搭建的。如果在服务器上可搭建Wordpress等动态博客并绑定域名，同时可以给ttrss绑定域名。（国内域名需要备案，我嫌麻烦就一直没去）
