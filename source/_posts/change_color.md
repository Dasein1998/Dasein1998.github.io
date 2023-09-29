---

title: 修改个人网站配色 
date: 2021-11-12 13:13:13
tags: 
- 折腾

---

这个网站的主题是cactus。其项目地址在：https://github.com/probberechts/hexo-theme-cactus
默认的配色我觉得已经很好看了。只是最近网上冲浪的时候总是看到和喔用同一个主题的博客，于是想着简单的修改一下。
于是翻开cactus主题的文件夹，看到/source/css/_colors/css文件夹中有对应的主题配色文件，于是我参照里面的配色文件给自己修改了一个。
```css
$color-background = #FCFAF2
$color-footer-mobile-1 = darken($color-background, 2%)
$color-footer-mobile-2 = darken($color-background, 10%)
$color-background-code = darken($color-background, 2%)
$color-border = #666
$color-meta = #666
$color-meta-code = lighten($color-meta, 10%)
$color-link = #7DB9DE
$color-text = #363533
$color-accent-3 = #666666
$color-accent-2 = #111111
$color-accent-1 = #EB7A77
$color-quote = #9E7A7A
$highlight = hexo-config("highlight") || "github"
```
效果参考博客效果。

更改字体大小：位置在`themes\cactus\source\css\_variables.styl`
默认大小为14。
```css
$font-size = 15px
```

# ref
[@font-face — Stylus 中文文档 | Stylus 中文网](https://www.stylus-lang.cn/docs/font-face.html)

