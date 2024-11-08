---
title: Obsidian笔记转为LogSeq笔记
date: 2024-08-18 14:14:14
tags:
my: ObsidiantoLogSeq
---
省流：
使用VScode的正则替换（点击搜索中 .* 这个按钮） 

```md
!\[\[((?<=\[\[).*\.\w{3,4})(\|\w{3})?\]\]
```

可以匹配诸如`![[Pasted image 20230829064601.png|325]]`，`![[Pasted image 20230829064601.png]]`格式的文本。

替换框里填

```md
![]($1)
```

则可转化为`![](Pasted image 20230829064601.png)`
既markdown标准图片格式。
用`![](/assets/$1)` 替换，则可转为`![](/assets/Pasted image 20230829064601.png)`，即LogSeq中的图片格式。

也可以反过来，使用如下表达式

```md
!\[\]\((assets/)((?<=).*\.\w{3,4})(?=\))\) 
```

来匹配 `![](assets/Pasted image 20230829064601.png)`
替换为 `![[$2]]` 则可得到  `![[Pasted image 20230829064601.png]]`

---
一直都想入门正则，但每次都感觉太复杂而不知道从哪下手。
只有自己真的有需求，且“狠下心来”直接解决问题的时候，才发现问题并没有那么复杂，正则也不是太难。

## 具体解析
使用`\w.*(?=\|\d{3}\]\])`
可以匹配
```
![[image-20240406082314566-一个例子.png|250]]
```
中的`image-20240406082314566-一个例子.png`。

替换为 `![]($1)` ，则将上述文字转为`![](image-20240406082314566-一个例子.png)`
其中$1为之前第一个括号里匹配的字符。
因为`\w`，为匹配字母、数字、以及下划线和汉字。
`.*`中，`.`为匹配除了换行符以外的任意字符，*为匹配尽可能多的字符。
`\w.*`是指，匹配以字母、数字、以及下划线和汉字为开头的，尽可能多的字符。
`(?=\|\d{3}\]\])`中，`\|\d{3}\]\]`中，`\|`为|的转义，\d{3}为匹配长度为三的任意数字，`\]`匹配]，于是`\|\d{3}\]\]`表示匹配`|123]]`这种形式的字符串。
(?= )表示匹配到`\|\d{3}\]\]`之前的位置，既`|123]]`这种形式的字符串之前的位置。
同理，使用`(\w.*(?=\]\]))`，表示匹配`]]`这种形式的字符串之前的位置。
(?<=\[\[)表示匹配到`\[\[`之后的位置，既匹配`[[`这种形式的字符串之后的位置。
这样，我们组合起来，

!\[\[((?<=\[\[)(\w.*\.png(?=\]\])))\]\]
写成这样，就是匹配
![[image-20240406082314566-一个例子.png]]
但提出image-20240406082314566-一个例子.png为$1，然后转为

```
![](image-20240406082314566-一个例子.png)
```

其中 !\[\[ 匹配![[
```
((?<=\[\[)(\w.*\.png(?=\]\])))
```
为$! 匹配
image-20240406082314566-一个例子.png
(?<=\[\[)表示以[[开头的后续
\w.*\.png \w为匹配字母或数字或下划线或汉字，.*为贪婪匹配，匹配尽可能多的次数；.png匹配 .png。

(?=\]\])表示以]]为结尾的后续
```
!\[\[((?<=\[\[)  (.*\.\w.*(?=\]\])))\]\]
```

.*\.\w.* 就直接匹配所有以 字符.字符 结构的。

 ```
!\[\[((?<=\[\[)(.*\.\w{3,4}(?=\]\])))\]\]
```

但问题就。。
![[image-20240406082314566-一个例子.png|250]]
这种在]] 前有|250的，咋匹配呢？
加一个匹配|250的部分
!\[\[((?<=\[\[).*\.\w{3,4})(\|\w{3})?\]\]
其中(\|\w{3})?表示匹配|250 0次或1次
```
![](assets/$1) 
```
反过来，也可以
!\[\]\((assets/)((?<=).*\.\w{3,4})(?=\))\)
或者在没有asset的字母时
用!\[\]\(((?<=).*\.\w{3,4})(?=\))\)
替换为


## Reference
[正则表达式中圆括号的用法--也叫后向引用 - dosir - 博客园](https://www.cnblogs.com/djh-create/p/5964986.html)
[正则表达式30分钟入门教程](https://deerchao.cn/tutorials/regex/regex.htm)
[正则表达式可视化: Regular Expression Tester and Visualizer](https://devtoolcafe.com/tools/regex#!flags=img&re=)
