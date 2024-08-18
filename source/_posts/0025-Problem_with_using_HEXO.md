---

title: 记录HEXO使用中踩的坑
date: 2021-02-24 16:16:16
tags: 
- 折腾
categories: 
- 工具 
my: Problem_and_solution_with_using_HEXO
---
如果看过我前面的文章就可以发现，我能用文字的时候尽量不用图片，主要原因就是图片不好处理。
基于GitHub Pages搭建的博客要使用图片有两种方式：
1. 直接引用

   把图片放在source文件夹中，直接在md文档中引用。如

   ```
   [](path/to/picture.jpg)
   ```

2. 使用图床

   如我正在用的SM.MS图床。使用时只需要把图片上传到图床上（我使用的是PicGo），然后将图床生成的Markdown代码粘贴到文档里就行了。

问题：

1. 引用svg文件

   SM.MS图床不支持.svg文件 。而我又懒得找另一个图床了，所以就选择第一种方式。

   于是在博客的/sourse/文件夹建立了picture/文件夹，把图片放进去。然后仿照[如何优雅地在Hexo博客中嵌入SVG文件](https://www.wangfeng.pro/2019/01/%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E5%9C%B0%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E5%B5%8C%E5%85%A5SVG%E6%96%87%E4%BB%B6.html) 中，用html代码来引用svg文件。文件路径为：

   `/sourse/picture/picture.svg`  将其填入html代码则可

   ```
   <div class="post-svg-container">
       <object type="image/svg+xml" data="xxx.svg"></object>
   </div>
   ```


   2. 折叠代码
      比如我在[PowerShell折腾记录](https://Dasein1998.github.io/2021/02/23/powershell/)中展示了PowerShell的配置文件，但其内容太长以致于影响观感。于是想默认将其折叠，但是Markdown的语法不支持这个。搜一搜发现需要使用HTML的标记语言，模板如下：
```
<details>
 <summary>点击查看隐藏内容</summary>
​```powershell
Install-PackageProvider -Name NuGet -Force
Install-Module -Name PowerShellGet -Force
​```
</details>
```
其中powershell是标记代码的类型，为了让`highlightjs`能够正确的渲染它。

使用效果如下：

<details>
 <summary>点击查看隐藏内容</summary>
```powershell
Install-PackageProvider -Name NuGet -Force
Install-Module -Name PowerShellGet -Force
```
</details>

