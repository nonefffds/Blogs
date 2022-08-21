---
title: 潦草学习 AppleScript：AppleScript 初上手
date: 2020-04-10 00:57:13
tags: 
- AppleScript
- Coding
- macOS
---
# 潦草学习 AppleScript：AppleScript 初上手

*潦草，即为不认真之意。*

## 前言

这两天接触了一下 AppleScript，发现还挺简单的，于是开始稍微学习了一下，正如标题所说——潦草学习，大概就是学到哪算哪的感觉。

这篇文章将会说明 AppleScript 的初期设置和一些基础语法，然后将会用一个用于上网课偷懒的小程序（尽量不要跟着模仿）作为对微信和 Safari 的控制演示。

<!-- more -->

## 准备

你需要准备的：
* 一台搭载 MacOS 的电脑（白果黑果皆可）和配套键鼠
* 脑子

关于要使用的 IDE，其实在苹果系统中已经自带了。你可以直接使用系统内的“脚本编辑器”，也可以使用你现有的更现代的 IDE，例如本文中使用的 VSCode 加 AppleScript 插件。
VSCode 的安装你可以直接在命令行使用`brew cask install visual-studio-code`（需要 homebrew，可参考本博客上一篇文章）进行安装。
在 VSCode 内，直接在左侧的功能键点插件然后搜索 AppleScript 选择第一项安装即可。
![](15864523571150.png)

然后就没什么能准备的了，如果需要运行写好的 AppleScript 直接按 Shift + F5 就能直接运行了（单按 F5 则是 debug，或者直接在选项栏里点 Run 选前两项运行）。

## AppleScript 基础语法

我的最初的 AppleScript 入门来自于[少数派的这篇文章](https://sspai.com/post/46912)，推荐看一下这篇文章，看完后大概会对 AppleScript 有一个最基础的了解。

以下，是一个用 AppleScript 写的 Hello World 程序：

```
set FirstProgram to "Hello World"
```

运行就会得到 "Hello World" 的结果，然而这句代码的含义仅仅是设定一个名为 FirstProgram 的变量，内容是  "Hello World" 而已，其中并没有使用到其他语言中可能常用的 print, println, printf 之类的打印输出命令，因为我在 AppleScript 中一直没找着这个对应的命令是什么... 如果有人知道希望能评论告诉我一下，谢谢...

如果要调出一个窗口显示 Hello World，则可以这么写：


```
display dialog ("Hello World")
```

运行之后就会得到一个这样的窗口
![](15864532386213.png)
以上です。

这就是最基本的语法了，那我们来练个习吧！
练习1.1: 请试用 AppleScript 写出检验 ISBN 号是否正确的程序
（并不）

...作为一个用 Project 学习的其实也差不多了，不过这次我们的目标是用 AppleScript 做一个自动微信签到并且登录网课的偷懒工具，接下来我会通过写这个工具来慢慢解释每句用到的东西的意义。

注：在 AppleScript 内你可以使用 `--` `#` `//`等符号作为注释开头（和结尾），苹果是推荐你用第一种的，但是在接下来的内容中为了方便我自己，我都会用井号进行注释。
## 实例

我们的需求功能是：

* 在微信班级大群里发送每天早上的报道
* 打开网课页面并进入网课

### 最终代码 Showcase
以下是本例的最终代码：

```
tell application "WeChat" to activatetell application "System Events"	tell process "Wechat"		keystroke "fandishengdao" 		keystroke "1"		key code 36	end tell	delay 6end telltell application "Safari" to activatetell application "Safari"	tell window 1		set current tab to (make new tab with properties {URL:"https://brunhild.cn/demo/study_center/schedule.vpage"})		delay 300		tell application "System Events"			click at {1587, 507}			delay 60		end tell	end tellend tell
```
### 在微信班级大群里发送每天早上的报道

首先，是第一行`tell application "WeChat" to activate`，它的作用是告诉(tell)微信("WeChat")这个应用程序(application)让(to)他打开(activate)。
![这是一个小号](15864544216139.png)
然后我们将默认打开的那个页面是你要发送的大群页面（微信并没有相关 API 检测你开的是哪个聊天页面，不过你可以使用微信置顶的方式保证你打开的是你要发送的页面，但更重要的原因是，我懒）。

然后我们就可以报道了。
接下来我们要调用 `System Events` 来帮我们输入报道消息并发送了。

```
tell application "System Events" #调用应用 System Events	tell process "Wechat" #告诉进程微信		keystroke "fandishengdao" #模拟按键输入"fandishengdao"，你可以根据需要改成你自己的名字，在本例中范迪盛并不是我的真名		keystroke "1" #模拟按键输入"1"，即输入法选择第一项		key code 36 #按下回车	end tell #结束“告诉”	delay 6 #延迟六秒钟end tell #结束“告诉”
```
![](15864552216000.png)

你可能在此会问了，其中的 keystroke 既然可以模拟输入，那为什么不干脆直接输入中文得了，而是要在有输入法的前提下调用输入法进行操作呢？
非常简单，因为如果你 keystroke，它会输出乱码。所以必须绕一步路。（除非你们群用英语答到）

然后关于你键盘上的所有 key code，可以参考一下[这个链接](https://eastmanreference.com/complete-list-of-applescript-key-codes)。

至于最后为什么有个 `delay 6`，是因为如果不稍微延迟一下，接下来要运行的 Safari 窗口可能会挡住原来的窗口（尽管这并不是个问题，不过我就是加了，这个命令我也建议记一下）。

OKay，答完“到”了，我们可以开始“上”网课了。

### 打开网课页面并进入网课

首先我们需要呼出 Safari：`tell application "Safari" to activate`
然后接下来就是行云流水的：
```
tell application "Safari"	tell window 1 #告诉第一个窗口		set current tab to (make new tab with properties {URL:"https://brunhild.cn/demo/study_center/schedule.vpage"}) 
		#设定目前的tab到（创建一个带有属性：链接 的新tab，链接是：～）
		#此处的链接是一个虚假的链接，域名并没有使用实际的链接		delay 300 #延迟300秒用来等待网页加载，总不可能300秒还加载不出来吧		tell application "System Events"			click at {1587, 507}#点击开始上课的按钮的坐标			delay 60 #延迟60秒			end tell	end tellend tell
```
其中你觉得要是有可能没点上就可以再加一行`click at`，其中取得坐标点要根据实际网页的位置进行修改。![截屏2020-04-10 上午2.17.00](%E6%88%AA%E5%B1%8F2020-04-10%20%E4%B8%8A%E5%8D%882.17.00.png)
然后就可以自动开始上课了。

那接下来就有一个问题了，如何才能让这个脚本自动运行呢？

### 使脚本自动运行

严格说来这部分已经不属于 AppleScript 范畴了，不过我还是大概说一下。

你可以使用 Crontab，launchd 和 iCal 进行脚本的自动运行。我使用的是 iCal，也就是系统自带的那个日历软件。

使用 iCal 设置定时运行脚本的方法可以参考[少数派的这篇文章](https://sspai.com/post/41735)

## 总结

也没什么好总结的，~~就是水一片文章~~，本文中的所有代码你可以从[这个GitHub Repo](https://github.com/nonefffds/broadcast-classroom-lazy) 里找到。我们下篇文章再见！