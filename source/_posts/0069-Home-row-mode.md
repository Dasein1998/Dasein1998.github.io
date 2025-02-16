---

title: 使用Home-row-mode
date: 2024-11-11 14:14:14
tags: 
- 折腾
categories: 
- 工具
my: Home-row-mode
---
Home row指的是asdfghjkl这一行，因为手在使用键盘的时候，80%以上的时间都是放在这一行上的，于是叫做Home row。
还没有一个合适的中文翻译，只好叫主行。
那Home row mode指的就是，主行模式。
从所周知，使用软件是离不开Ctrl、Shift、Alt等控制键的。
Home row mode就是想让人能把手放在主行上，就能使用控制键。
它们做法很简单：
把A S D F  J K L ; 这八个键，短按保持不变，长按改为控制键。
![homerowmode](https://cdn.sa.net/2024/11/10/BVxWwAt8fLOC74I.png)

这样的话，在大多数清况下使用控制键时，手指就不用离开主行。
减小了手指所需移动范围。
缺点也很明显：
需要熟悉短按和长按的节奏，以及要重新肌肉记忆新的快捷键按键方式。
把对更大范围的键位的肌肉记忆，改为了对小范围键位的组合记忆。
比如Ctrl-c 变为了;-c。（我把;改为了Ctrl）

目前要使用home row mode有两种方法：

1. 键盘固件支持

如果使用的是QMK、ZMK等开源固件，那可以在固件里改。具体参考：[A guide to home row mods](https://precondition.github.io/home-row-mods#cags)
文章内的修改方式。

2. 软件改键

如kanata、KMonad 等全平台改键软件。
上文中提供了Kmonad的方法，但Kmonad在windows上的安装较为繁琐，于是我选择了Kanata作为改键软件来实现Home-row-mode。

## Kanata 来实现home row mode

Kanata的安装很简单：

```powershell
scoop install kanata
```

运行之前，需要建立一个配置文件，在windows下的配置文件位于：`C:\Users\username\AppData\Roaming\kanata\kanata.kbd`
（如果不存在这个位置，则需要新建这个文件夹和文件）
Kanate的Github仓库里自带一个Home-row-mode的配置文件：[kanata/cfg\_samples/home-row-mod-advanced.kbd at main · jtroo/kanata](https://github.com/jtroo/kanata/blob/main/cfg_samples/home-row-mod-advanced.kbd)
可以下载后复制到上面的配置文件中。
然后在命令行中输入kanata，既可使用了。
以下既为官方提供的配置文件，其中asdfjkl;对应的控制键，可以由用户自行修改。

```lisp
;; Home row mods QWERTY example with more complexity.
;; Some of the changes from the basic example:
;; - when a home row mod activates tap, the home row mods are disabled
;;   while continuing to type rapidly
;; - tap-hold-release helps make the hold action more responsive
;; - pressing another key on the same half of the keyboard
;;   as the home row mod will activate an early tap action

(defcfg
  process-unmapped-keys yes
)
(defsrc
  a   s   d   f   j   k   l   ;
)
(defvar
  ;; Note: consider using different time values for your different fingers.
  ;; For example, your pinkies might be slower to release keys and index
  ;; fingers faster.
  tap-time 200
  hold-time 150

  left-hand-keys (
    q w e r t
    a s d f g
    z x c v b
  )
  right-hand-keys (
    y u i o p
    h j k l ;
    n m , . /
  )
)
(deflayer base
  @a  @s  @d  @f  @j  @k  @l  @;
)

(deflayer nomods
  a   s   d   f   j   k   l   ;
)
(deffakekeys
  to-base (layer-switch base)
)
(defalias
  tap (multi
    (layer-switch nomods)
    (on-idle-fakekey to-base tap 20)
  )

  a (tap-hold-release-keys $tap-time $hold-time (multi a @tap) lmet $left-hand-keys)
  s (tap-hold-release-keys $tap-time $hold-time (multi s @tap) lalt $left-hand-keys)
  d (tap-hold-release-keys $tap-time $hold-time (multi d @tap) lctl $left-hand-keys)
  f (tap-hold-release-keys $tap-time $hold-time (multi f @tap) lsft $left-hand-keys)
  j (tap-hold-release-keys $tap-time $hold-time (multi j @tap) rsft $right-hand-keys)
  k (tap-hold-release-keys $tap-time $hold-time (multi k @tap) rctl $right-hand-keys)
  l (tap-hold-release-keys $tap-time $hold-time (multi l @tap) ralt $right-hand-keys)
  ; (tap-hold-release-keys $tap-time $hold-time (multi ; @tap) rmet $right-hand-keys)
)
```

## Reference
[jtroo/kanata: Improve keyboard comfort and usability with advanced customization](https://github.com/jtroo/kanata)

[A guide to home row mods](https://precondition.github.io/home-row-mods#cags)