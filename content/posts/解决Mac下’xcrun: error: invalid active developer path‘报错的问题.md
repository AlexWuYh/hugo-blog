---
title: 解决Mac下"xcrun error invalid active developer path"报错的问题
date: 2022-11-01
tags:
    - MacOS

categories:
    - Mac
---

最近Mac推送了最新的系统版本`Ventura`，在升级到该版本之后发现，在终端中使用`git`、`python`等命令的时候会报错，报错信息如下：

`xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun`

![](https://cdn.jsdelivr.net/gh/alexwuyh/pic-host@master/photo/202211011724578.png)

在网上搜索了下解决办法，这个问题可能是因为`xcode`的版本和新系统不兼容、不适配导致的，这个时候只需要更新一下`xcode`版本即可解决。

解决办法如下：

- 打开`terminal`终端，执行以下命令：

```bash
xcode-select --install
```

![](https://cdn.jsdelivr.net/gh/alexwuyh/pic-host@master/photo/202211011727762.png)

- 执行命令后，系统会弹出一个下载确认框，点击`确认`按钮开始下载，下载时间大概需要1分钟(具体以实际网络环境速度为准)
- 下载完成后会开始安装，这个时候弹窗中提示的剩余时间可能显示为几十或上百小时，不要慌，实际上可能也就半小时左右就能安装完成😂

安装完成后，再执行`git`、`python`等命令即可正常使用了
![](https://cdn.jsdelivr.net/gh/alexwuyh/pic-host@master/photo/202211011732718.png)