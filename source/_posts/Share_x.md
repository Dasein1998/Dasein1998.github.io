---
title: Share X配合SM.MS图床实现自动上传图片
date: 2022-07-25 14:14:14
tags:
  - 折腾
  - 自动化
  
categories:
  - 工具
---
之前买了永久SM.MS图床的会员，然后用的是picgo来上传图片。但每次需要用截图软件截图，然后打开picgo上传，就感觉不大方便。于是配置了Share X来截图和自动上传图片。
Share X的官网：[ShareX - The best free and open source screenshot tool for Windows](https://getsharex.com/)
Share X自带的图片上传服务是没有SM.MS的：![](https://vip2.loli.io/2022/07/26/zuAUVDYG6vdOpJI.png)
但是官方的GitHub仓库里是有其他各种图床的配置文件的：[ShareX/CustomUploaders: ShareX custom uploaders](https://github.com/ShareX/CustomUploaders)
从中找到SM.MS的配置文件：[CustomUploaders/sm.ms.sxcu at master · ShareX/CustomUploaders](https://github.com/ShareX/CustomUploaders/blob/master/sm.ms.sxcu)
```json
{
  "Version": "12.4.1",
  "DestinationType": "ImageUploader",
  "RequestMethod": "POST",
  "RequestURL": "https://sm.ms/api/v2/upload",
  "Headers": {
    "User-Agent": "ShareX/12.4.1",
    "Authorization": "Your API Token goes here"
  },
  "Body": "MultipartFormData",
  "Arguments": {
    "format": "json"
  },
  "FileFormName": "smfile",
  "URL": "$json:data.url$",
  "DeletionURL": "$json:data.delete$"
}
```
复制到剪贴板，然后打开ShareX中![](https://vip2.loli.io/2022/07/26/9PzQuWyYepcgOj3.png)
![](https://vip2.loli.io/2022/07/26/yBtZfndj1gJkcIx.png)
选择剪贴板，然后再用自己SM.MS的token，填入"Authorization"中。
最后，将URL替换为`![]({json:data.url})`，这样上传后可以得到Markdown格式的链接。

