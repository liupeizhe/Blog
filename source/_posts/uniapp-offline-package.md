---
title: uniapp-离线打包apk
date: 2020-11-21
tags: ['uniapp', 'Android']
---

# 一、基础环境
+ 下载Android Studio配置好环境。
+ 下载[Android离线版SDK](https://nativesupport.dcloud.net.cn/AppDocs/download/android?id=android-%e7%a6%bb%e7%ba%bfsdk-%e6%ad%a3%e5%bc%8f%e7%89%88)并解压。
+ 导入`UniPlugin-Hello-AS`项目，导入后需下载依赖包。

# 二、生成本地打包app资源
HBuilder X > 发行 > 原生App-本地打包 > 生成本地打包app资源

![生成本地打包app资源](/images/uni-app/01.jpg)

导出成功

![导出成功](/images/uni-app/02.jpg)

# 三、SDK配置

点击路径打开文件夹，复制`__UNI__37D3898`文件夹到：

UniPlugin-Hello-AS > app > src > main > assets > apps下，没有apps文件夹自己创建一个就好。

打开UniPlugin-Hello-AS > app > src > main > assets > data > dcloud_control.xml，将appid替换成你自己的。

![截图](/images/uni-app/03.jpg)

![修改appId](/images/uni-app/04.jpg)

*appId每个都不一样，改成你自己的就好*

# 三、打包
Android Studio > Build > Build Bundle / APK > Build APK
点击开始打包

![Build APK](/images/uni-app/05.jpg)

打包成功，右下角会弹出提示，点击`locate`，文件夹里的`app-debug.apk`就是我们要的了。

![success](/images/uni-app/06.jpg)

# 四、修改应用名称、应用图标、启动图
1. 修改应用名称

打开UniPlugin-Hello-AS > app > src > main > res > values > strings.xml，将名称改成你想要的就好了

![截图](/images/uni-app/07.jpg)
![截图](/images/uni-app/08.jpg)

然后检查下res > AndroidManifest.xml，按图修改

![截图](/images/uni-app/09.jpg)

2. 修改应用图标、启动图

如图，创建文件夹，将你的图片按不同分辨率放到不同文件夹里，一一对应，所有图片名称都是一样的
*忽略push.png*

![截图](/images/uni-app/10.jpg)

— END —
