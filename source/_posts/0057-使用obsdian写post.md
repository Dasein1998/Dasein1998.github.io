---

title: 使用obsdian写blog
date: 20221029 12:59:12
tags: 
- 
categories: 
my: Write_blog_by_obsidian
---
# 安装ObsidianShell
最近一直在使用 [Obsidian](https://obsidian.md/)这款笔记软件来记笔记，安装了大量的插件之后，obsdian 逐渐变得很顺手了。
然后前段时间，在小众软件上看到这个文章：[ObsidianShell：关联 .md 文件到 Obsidian - 发现频道 - 小众软件官方论坛](https://meta.appinn.net/t/topic/32019)。方便在Windows中，使用obsdian来对.md文件进行编辑。 
作者将其开源在 GitHub 上，仓库地址为[Chaoses-Ib/ObsidianShell: Associate Markdown files with Obsidian](https://github.com/Chaoses-Ib/ObsidianShell)
下载安装之后，可以在打开方式中设置。
![|425](https://vip2.loli.io/2022/10/29/tK3cz26mBrDfsYA.png)

软件的配置文件为：ObsidianCLI.exe.Config。位于软件安装位置`C:\Program Files\Chaoses Ib\ObsidianShell\ObsidianCLI.exe.Config`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
      <add key="OpenMode" value="VaultFallback" />
      <add key="MarkdownFallback" value="E:\Program Files\Typora\Typora.exe" />
      <add key="MarkdownFallbackArguments" value="{0}" />
      <add key="RecentVault" value="C:\path\to\Recent" />
      <add key="RecentLimit" value="100" />
    </appSettings>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.8" />
    </startup>
</configuration>
```
它有三种模式：-   VaultFallback（默认)  VaultRecent 和 Recent 。
默认是 VaultFallback。即检测文件是否在 ob 的库里，若是，则用 ob 打开这个库中的这个文件。若否，则用 `<add key="MarkdownFallback" value="E:\Program Files\Typora\Typora.exe" />`设定的编辑器打开。    这里可以设置自己喜欢的其他编辑器。
VaultRecent 则：检测文件是否在 ob 的库里，若是，则用 ob 打开这个库中的这个文件。若否，则将文件链接到自定的recent文件夹中，然后打开。
Recent 则：则将文件链接到自定的 recent 文件夹中，然后打开。

！需要注意的是，使用 Recent 以及 VaultRecent 模式，需要先打开 ob 自定义一个 obsdian 的仓库，名称随意，可以事先在里面放自己的.obsdian 配置文件。然后用路径替换掉 `<add key="RecentVault" value="C:\path\to\Recent" />` 中的 `C:\path\to\Recent`。
然后保存配置文件即可。
！第二点需要注意的是：安装在c盘的话，需要管理员权限才能保存文件。

---
# 使用obsdian编辑blog
## 使用templater
博客使用的是HEXO的框架，博客文档默认存放在`\source\_post`文件夹中。
ObsidianShell 是使用软链接的方式，将打开的目录链接到了上面定义的 RecentVault中。
于是，打开 obsdian 之后，是可以直接使用 obsdian 来管理 `\source\_post` 文件夹中的文章的。
HEXO 的博客，其文章最开始都会使用 yaml 的语法，定义 md 文件的 Front-matter。
```yaml
---

title: 
date: 
tags: 
- 
categories: 
- 
---
```
这时候，使用templater插件，定义一个模板，比如
```yaml
---

title: 
date: <% tp.file.creation_date ("YYYYMMDD HH:mm:ss") %>
tags: 
- 
categories: 
- 
---

# Reference
```
就可以很方便生成 Front-matter，可以利用 templater 插件自动添加时间，标题，以及各种所需参数。

同样，可以用上ob里其他大量插件来辅助博客的写作。比如tag wragler修改tag，pandoc导出，Autocomplete自动补全等。 以及，当自己做笔记并且想更新博客的时候，也可以直接通过ob的库来打开recent文件夹来更新博客，使用起来十分流畅。
有必要的话，甚至可以使用ob来跑脚本，实现HEXO博客的自动更新。
# Reference
[Chaoses-Ib/ObsidianShell: Associate Markdown files with Obsidian](https://github.com/Chaoses-Ib/ObsidianShell)
[SilentVoid13/Templater: A template plugin for obsidian](https://github.com/SilentVoid13/Templater)