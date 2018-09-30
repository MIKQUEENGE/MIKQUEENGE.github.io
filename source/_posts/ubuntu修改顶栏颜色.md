---
title: ubuntu修改顶栏颜色
toc: false
date: 2018-09-29 19:14:01
categories:
- methods
tags:
- Ubuntu
---

编辑shell主题的css文件，比如我的shell主题是`Vimix-Beryl`：

```shell
sudo gedit /usr/share/themes/Vimix-Beryl/gnome-shell/gnome-shell.css
```

打开之后搜索`top bar`，会看到这样一段：

```css
/* TOP BAR */
#panel {
  background-color: rgba(0, 0, 0, 0.6);
  /* transition from solid to transparent */
  transition-duration: 250ms;
  font-weight: bold;
  height: 32px;
}
```

按照自己的喜好修改`background-color`即可^_^



补充：修改完可能并不能直接看到效果，可以在tweaks里先切换成别的主题再换成修改了的主题。



再补充：因为我是把透明度调到了大概0.2左右，导致使用hide top bar扩展时使用鼠标触发top bar后几乎看不清，可以往下找一下`#panel.solid`，把`background-color`改成这样：

```css
#panel.solid {
  background-color: rgba(0, 0, 0, 0.5);;
  /* transition from transparent to solid */
  transition-duration: 250ms;
  background-gradient-direction: none;
  text-shadow: none;
}
```

