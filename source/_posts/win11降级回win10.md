---

title: win11降级回win10
date: 2022-07-24 22:22:22
tags: 
- 折腾
categories: 
- 
---
在远景论坛上看到这个帖子[简单聊聊我如何从Windows11平滑降级回Windows10-远景论坛-微软极客社区](https://bbs.pcbeta.com/viewthread-1904414-1-1.html)，然后苦于win11相比win10太卡顿了，就用帖子的办法尝试降级回win10。简单记录一下遇到的问题吧
在win11系统之中，如果直接加载win10的镜像，是只有：**不保留个人文件和数据**这个选项的。
而教程中则提供一种方法：绕过windows安装过程中的版本检测
其原理是：微软目前的采取的版本检测方法是直接检测两个版本号字符串，看目标系统的版本号是否大于当前系统的版本号。

首先下载一个win10镜像，然后用解压软件解压win10镜像，
然后按照文中的办法，先下载一个反汇编工具，如IDA：[IDA Freeware](https://hex-rays.com/ida-free/)。
用这个工具打开windows镜像中`source\setupcompact.dll`文件。
查找到ConX::Setup::Common::CWindowsVersion::IsLaterThan。
可以搜索IsLaterThan，也可以在IDA左边的文件树中找到这个函数。
在该函数的末尾有两个label，分别是返回0和返回1，这里直接全部返回0，把mov eax, 1改成mov eax, 0。
保存对DLL的修改，替换原本的source\setupcompact.dll文件。
主要用到的是IDA的patch program 功能，包括：Change byte：通过修改字节来修改程序；Apply patches to input file：将之前修补的字节应用回输入文件；
然后运行setup.exe就可以保留用户文件升级了。

升级之后，遇到的第一个问题：进系统就黑屏 只有鼠标可以显示，应用可以打开但无法显示。
解决方案：进PE，这里推荐微PE：[微PE工具箱 - 下载](https://www.wepe.com.cn/download.html)
删除C:\ProgramData\Microsoft\Windows\AppRepository目录下的所有StateRepository* 文件。
然后会发现，可以进系统了，这时会遇到下一个问题：
系统提示开始界面无法使用，此时需要的是在powershell中运行：
`add-appxpackage -register "C:\Windows\SystemApps\*\AppxManifest.xml" -disabledevelopmentmode`来恢复系统应用，然后重新登录用户，此时可以正常进系统使用了。
然后会发现现在系统里没有应用商店，则需要运行：
`add-appxpackage -DisableDevelopmentMode -Register "C:\ProgramData\Microsoft\Windows\AppRepository\*\AppxManifest.xml" -verbose`
会出现很多warning和error，重复两次，可以看到恢复了绝大多数的uwp应用，且应用数据依旧存在。
最后在powerhsell下运行`add-appxpackage -DisableDevelopmentMode -Register "C:\Program Files\WindowsApps\*\AppxManifest.xml" -verbose`恢复uwp应用。
自此，已经将win11 几乎完美降级回了win10，暂时没有遇到新的bug。降级回来的感受就是，系统真的流畅了四五倍。。


