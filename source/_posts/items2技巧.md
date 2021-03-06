---
title: items2技巧
date: 2018-04-13 17:49:58
tags:
---

+ 标签
+ 分屏
+ 快捷键

<!--more-->

# items2 技巧

## 标签
```
新建标签：command + t

关闭标签：command + w

切换标签：command + 数字 command + 左右方向键

切换全屏：command + enter

查找：command + f
```

## 分屏

```
垂直分屏：command + d

水平分屏：command + shift + d

切换屏幕：command + option + 方向键 command + [ 或 command + ]

查看历史命令：command + ;

查看剪贴板历史：command + shift + h
```



## 其他

```
⌘ + shift + h 会列出剪切板历史

清除当前行：ctrl + u

到行首：ctrl + a

到行尾：ctrl + e

搜索命令历史：ctrl + r

删除光标之前的单词：ctrl + w

清屏：ctrl + l

自带有哪些很实用的功能/快捷键

⌘ + f 所查找的内容会被自动复制

输入开头命令后 按 ⌘ + ; 会自动列出输入过的命令

可以在 Preferences > keys 设置全局快捷键调出 iterm，这个也可以用过 Alfred 实现
```


## 选中即复制

### 选中即复制

#### iterm2 有 2 种好用的选中即复制模式。

- 一种是用鼠标，在 iterm2 中，选中某个路径或者某个词汇，那么，iterm2 就自动复制了。 　　
- 另一种是无鼠标模式，`command+f`,弹出 iterm2 的查找模式，输入要查找并复制的内容的前几个字母，确认找到的是自己的内容之后，输入 `tab`，查找窗口将自动变化内容，并将其复制。如果输入的是 `shift+tab`，则自动将查找内容的左边选中并复制。

### 自动完成

输入打头几个字母，然后输入 `command ;` iterm2 将自动列出之前输入过的类似命令。 

### 剪切历史

输入 `command+shift+h`，iterm2 将自动列出剪切板的历史记录。如果需要将剪切板的历史记录保存到磁盘，在 `Preferences > General > Save copy/paste history to disk` 中设置。