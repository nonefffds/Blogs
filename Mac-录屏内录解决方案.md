---
title: Mac 录屏内录解决方案
date: 2020-04-11 17:44:53
tags: 
- macOS
- tools
---
# Mac 录屏内录解决方案
*Yet simple enough, moreover fully functioned.*
## 前言

18年最后一天在b站投了个[如何用 Logic Pro X 调音的小教程](https://www.bilibili.com/video/BV16t41167Gu)，然后一直也有人在问 macOS 是用的什么软件录屏和录制系统声音的，因此在这写篇文章写一下如何在 macOS 实现屏幕和系统声音的录制。

<!-- more -->

在这里我要介绍一下 Background Music，这是一款能让你享受到和 Windows 上一样的分程序独立控制音量的软件，而且由于注册了系统内的虚拟音频 midi 设备，也可以对输入的音频进行转发（hijack）。这个软件在[少数派的这篇文章](https://sspai.com/post/52588)中也有提及。

## 准备

* 苹果 macOS 系统（这篇文章仅支持 macOS，我目前使用的版本是 Catalina 10.15.4），如果你是 macOS 10.1 的系统（即 Mojave），你可以直接按 Command + Shift + 5 组合键直接开启录屏，方法在文章最后
* 最新版 OBS，你可以使用 HomeBrew 安装：`brew cask install obs`，或者在[官网下载安装文件](https://obsproject.com)
* 最新版 Background Music，你也可以使用 HomeBrew 安装 `brew cask install background-music`，或者在 [GitHub 下载安装包](https://github.com/kyleneideck/BackgroundMusic/releases)
* 脑子

**注：如果系统内并没有 HomeBrew，请参考[少数派的这篇文章](https://sspai.com/post/42924)**
## 操作


安装 Background Music 时，你可能需要在 `系统偏好设置->安全性与隐私->通用`里点击允许安装才能正常安装。（如果你使用 HomeBrew 进行安装，命令行可能会通知你去设置里点击）

安装完 Background Music 之后，你需要在 `系统偏好设置->声音->输出` 中选择 Background Music。

![](15866287048477.png)
然后打开导航栏中的 Background Music 图标![](15866287392458.png)，在 Output Device 下面的音频设备中点击你需要进行监听的外部设备（例如耳机，声卡）。![](15866288186989.png)

### 使用 OBS 录制
接下来打开录屏软件，例如 OBS：
![](15866291699001.png)
打开设置->音频，将任意一个麦克风/辅助音频选项选为 Background Music，然后点击确定，你就可以在 OBS 主页的最下方的混音器中的看到你的内录用的麦克风了。![](15866292641899.png)
接下来你可以在电脑中放一首歌来测试一下看混音器里对应的音量条有没有动，或者录制一下再回放听一下有没有声音。
### 使用 macOS 自带录屏录制（需 macOS Mojave 及更高版本系统）
然后是针对 macOS 内置录屏的设置，同样也非常简单。
点击任意的录屏选项（部分录屏/录制整个屏幕）后，在选项中的麦克风点击 Background Music 就可以了。
![](15866294910730.jpg)

## 总结
大概就是这样了... 
之前我是用的 Soundflower 来进行的 hijack，然而 Soundflower 现在已经 deprecated 了，而且正好我也有调节各个程序音量的需求，所以我就直接用 Background Music 了...
如果你需要其他非本文提供方式来进行类似的操作，可以参考[少数派的这篇文章](https://sspai.com/post/36155)
Overall，感谢阅读。

## 后记
本文已被作者修改后投稿至少数派，并被推荐至 Matrix 精选和少数派首页。感谢。