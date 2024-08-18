---
title: Obsidian转LogSeq
date: 2024-08-18 14:14:14
tags:
my: ObsidiantoLogSeq
---
省流：
使用VScode的正则替换，
```
!\[\[((?<=\[\[).*\.\w{3,4})(\|\w{3})?\]\]
```

可以匹配诸如`![[Pasted image 20230829064601.png|325]]`，`![[Pasted image 20230829064601.png]]`格式的文本。

替换框里填
```
![]($1)
```
则可转化为`![](Pasted image 20230829064601.png)`
既markdown标准图片格式。
用`![](assets/$1)` 替换，则可转为`![](assets/Pasted image 20230829064601.png)`，即LogSeq中的图片格式。

也可以反过来，使用如下表达式 
```
!\[\]\((assets/)((?<=).*\.\w{3,4})(?=\))\) 
```

来匹配 `![](assets/Pasted image 20230829064601.png)`
替换为 `![[$2]]` 则可得到  `![[Pasted image 20230829064601.png]]`

