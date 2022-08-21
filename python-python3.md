---
title: 在 shell 中使用 Python 命令时自动使用 Python3
date: 2021-09-13 21:15:13
tags: 
- Coding
- python
- Shell
---
# 在 shell 中使用 Python 命令时自动使用 Python3
*我知道很简单，但排查确实费时*
## 前言

近期在尝试在树莓派上装一个 maibot 时遇到了一些错误，后发现是因为默认的 python 版本问题，在 raspberry OS 环境下 shell 默认 python 版本是 python2。在这简要的写一下更改版本的方法。

<!-- more -->

## 方法

``nano ~/.bashrc`` (打开bash配置文件，如果用的zsh就是 .zshrc)

在末尾加入


``alias python=python3``

如果你需要用python3运行的软件需要使用sudo权限才能运行，则加一行

``alias sudo=sudo ``

## 参考

https://askubuntu.com/questions/320996/how-to-make-python-program-command-execute-python-3

https://askubuntu.com/questions/22037/aliases-not-available-when-using-sudo

