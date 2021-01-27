---

title: Manjaro+i3wm配置
date: 2021-01-26 18:18:18
tags: 折腾

---

Manjaro是基于Arch的衍生版。
初始装的是Manjaro-KDE，通过主题改成了Big Sur的样式。（有一说一，是真的好看）

装的是[Latter dock](https://github.com/KDE/latte-dock) 

> yay -S  latte-dock

---



然后发现KDE实在是太耗电了，于是换成Sway试试。

> yay -S sway

 Sway是基于Wayland的wm管理器，听说wayland的速度好于X11，且快捷键和使用方式和i3wm相同(可以使用和i3wm相同的配置文件)。于是装上后试了试，但是wayland的兼容性实在不行，只好放弃。（无法使用vivaldi浏览器。。）

---



i3是动态平铺窗口管理器，配置文件为纯文本文件。配置文件默认为

> ~/.config/.i3/config

在配置文件里可以配置快捷键 ，自启动，窗口平铺操作等

使用Rofi 当作应用启动器，polybar作为bar（状态栏）

> yay -S rofi polybar 

rofi使用默认的配置，polybar使用[Polybar Nord](https://github.com/Yucklys/polybar-nord-theme) 主题。

rofi生成默认的配置文件：

> mkdir -p ~/.config/rofi
> rofi -dump-config > ~/.config/rofi/config.rasi

然后在i3的config里加上主题参数，即可在启动的时候切换到所选的主题。

> bindsym $mod+d exec --no-startup-id rofi -show run -theme Arc-Dark

其中Arc-Dark为主题名称 ，rofi的主题目录为：

> /usr/share/rofi

### 配置Polybar Nord主题

```
git clone https://github.com/Yucklys/polybar-nord-theme ~/.config/polybar/
```

将其下载到 ~/.config/polybar/   （polybar的默认配置目录）

其中包含一个 launch.sh  文件，用于启动polybar ，可以选择light 或者dark主题。

> ./launch.sh light   

在~/.config/.i3/config的配置文件中写为：

>exec_always --no-startup-id $HOME/.config/polybar/launch.sh dark

使用之前需要安装一些依赖：

安装所需字体：

> yay -S ttf-font-awesome wqy-microhei

安装可选软件：

> yay -S dunst clipmenu xfce4-power-manager nm-applet  mpd alsa-utils  

Nord主题中有两个需要注意的地方：

1. 使用了今日诗词的api，会在屏幕的下方刷新诗词。可以自己去申请自己的api

[今日诗词api](https://v2.jinrishici.com/token) 然后替换掉~/.config/polybar/中的nord-down中作者的api  （在第75行左右）

2. 使用了[openweather](https://openweathermap.org)的天气api，需要自己申请api

   然后在polybar/scripts/openweathermap-fullfeatured.sh 中修改38，39行的api和地址

   默认的位置为BeiJing。



注意事项：nord-config中 需要修改默认的显示输出和默认的wifi硬件 有需要的还可以修改polybar的dpi

1. 第5行：如果也是13.1的2k屏幕建议修改为    dpi = 144 
2. 第6行 默认的输出是 eDP1（monitor = ${env:MONITOR:eDP1}）   但是我的显示器是eDP如果出现问题的话记的修改
3. 第66行 interface = wlp1s0  这里要换成自己的网卡  使用ifconfig 可以查看自己的网卡  通常有线网卡为eth开头，无线网卡为wlp开头





---

可选软件：vivaldi qqmusic-bin google-chrome typora thunderbird wps-office-cn joplin tlp 





