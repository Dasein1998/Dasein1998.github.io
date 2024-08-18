---

title: Vivaldi插件配置
date: 2021-09-03 14:14:14
tags: 
- 折腾
categories: 
- 工具
my: plugin_in_Vivaldi
---
Vivaldi会对浏览器的数据进行加密，不能通过备份数据文件来达到备份数据的目的。所以每次重装Vivaldi的时候，需要给插件进行一定的配置，于是在此记录插件需要进行的配置。

## [油猴脚本](http://www.tampermonkey.net/)

通过[Greasyfork](https://greasyfork.org/)下载脚本，常用的脚本有：

[Bilibili Evolved](https://cdn.jsdelivr.net/gh/the1812/Bilibili-Evolved@master/bilibili-evolved.user.js)一个哔哩哔哩的强化脚本，可以修改哔哩哔哩的主题等。
[知乎增强](https://greasyfork.org/zh-CN/scripts/419081-%E7%9F%A5%E4%B9%8E%E5%A2%9E%E5%BC%BA) 移除登录弹窗、默认收起回答、一键收起回答、收起当前回答/评论（点击两侧空白处）、快捷回到顶部（右键两侧空白处）、屏蔽用户 (发布的内容)、屏蔽关键词（标题/评论）、屏蔽指定类别（视频/文章等）、屏蔽盐选内容、展开问题描述、置顶显示时间、完整问题时间、区分问题文章、直达问题按钮、默认高清原图、默认站外直链。
[CSDN](https://greasyfork.org/zh-CN/scripts/378351-%E6%8C%81%E7%BB%AD%E6%9B%B4%E6%96%B0-csdn%E5%B9%BF%E5%91%8A%E5%AE%8C%E5%85%A8%E8%BF%87%E6%BB%A4-%E4%BA%BA%E6%80%A7%E5%8C%96%E8%84%9A%E6%9C%AC%E4%BC%98%E5%8C%96-%E4%B8%8D%E7%94%A8%E5%86%8D%E7%99%BB%E5%BD%95%E4%BA%86-%E8%AE%A9%E4%BD%A0%E4%BD%93%E9%AA%8C%E4%BB%A4%E4%BA%BA%E6%83%8A%E5%96%9C%E7%9A%84%E5%B4%AD%E6%96%B0csdn) CSDN去广告
[虎牙Plus](https://greasyfork.org/zh-CN/scripts/402279-%E8%99%8E%E7%89%99plus) 虎牙自动领取任务经验、开宝箱，复制直播流链接，简化页面，去广告，夜间模式，自动进入剧场模式,，订阅页面视频预览等

## Proxy SwitchyOmega配置[Autoproxy](https://github.com/aglent/autoproxy)

黑名单模式：SwitchyOmega 扩展里新建 switch profile 模式（适用于Firefox 57+和Chrome）
规则列表地址填入 https://git.io/gfw-list 或 https://raw.githubusercontent.com/aglent/autoproxy/master/gfwlist.pac
格式选择 autoproxy

白名单模式：规则列表地址填入 https://git.io/whitelistpac 或 https://raw.githubusercontent.com/aglent/autoproxy/master/whitelist.pac





