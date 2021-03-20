---
title: hexo添加卡通人物（看板娘）
date: 2020-08-30
tags: ['hexo']
---

输入如下命令获取live2d

```bash
npm install --save hexo-helper-live2d  
```
这可能需要点时间，耐心一点...

<!-- ![moments later...](/images/hexo-live2d/20200930145105.png) -->

下一步选择喜欢的卡通角色 ↓

[模型预览传送门](https://blog.csdn.net/wang_123_zy/article/details/87181892)

然后输入如下命令

```bash
npm install <packagename>
```

将`packagename`换成你挑选的模型名称即可。

最后一步 打开根目录下`_config.yml` 添加如下代码

```bash
live2d:
	enable: true
	scriptFrom: local
	model:
		use: live2d-widget-model-koharu #模型选择
	display:
		position: left #模型位置
		width: 150 #模型宽度
		height: 300 #模型高度
	mobile:
		show: false #是否在手机端显示
```

运行`hexo s`看下效果吧~

— END —

