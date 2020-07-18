---
title: ionic在Android下状态栏显示异常问题
date: 2020-01-09
tags: ['ionic']
---

ionic打包Android *.apk*后，手机安装运行发现状态栏黑漆漆一片，时间、手机信号等信息都不见了。

解决办法就是降低statusBar版本，找到项目根目录下`config.xml`,翻到最下面可以看到`cordova-plugin-statusbar`的版本，我们需要把版本降低到2.2.1。

运行命令：

```bash
ionic cordova plugin remove cordova-plugin-statusbar
ionic cordova plugin add cordova-plugin-statusbar@2.2.1
```

成功后进入`config.xml`查看版本是否切换成功了，然后重新打包测试即可，亲测可用。

— END —
