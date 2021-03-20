---
title: wangEditor 一款非常牛啤的富文本编辑器
date: 2021-03-20 11:20:50
tags: ['jQuery', '编辑器']
---

[wangEditor官方文档](https://www.wangeditor.com/)

最近用jQuery做一个小项目，需要用到富文本编辑器，找了一些但总是会出现各种小问题，直到我发现了wangEditor...

这款编辑器界面清爽、功能强悍，写起来简单易用、代码简洁，而且是开源免费的。

上代码：
## 生成编辑器

1. CDN引入`wangEditor.min.js`
```bash
<script src="https://cdn.jsdelivr.net/npm/wangeditor@latest/dist/wangEditor.min.js"></script>
```

2. HTML部分
```bash
<div id="editor"></div>
```

3. JS部分
```bash
$(document).ready(function() {
  // 设置不需要的菜单 看官方api
  editor.config.excludeMenus = [
    'video', // 视频
    ...
  ]
  // 编辑器默认会过滤掉粘贴过来的样式 如果需要样式手动将pasteFilterStyle设置为false
  editor.config.pasteFilterStyle = false
  editor.create()
})
```

保存代码即可看到效果了

![wangEditor](/images/wangEditor/1.jpg)

## 自定义图片上传
官方提供的上传，只需要在`editor.create()`前加上以下代码：
```bash
// 配置 server 接口地址
editor.config.uploadImgServer = '/upload-img'
```
加上后，上传图片里就出现了本地上传组件。

![本地上传](/images/wangEditor/2.jpg)

但是这种方式，对接口返回格式有严格要求，如果你的api跟官方不一致，会导致上传失败，这时候要自己实现图片上传。
在`editor.create()`前加上以下代码：

```bash
// 加这一行代码是因为要让上传本地图片组件显示
editor.config.uploadImgShowBase64 = true
/**
  * 自定义上传图片
  * resultFiles: 上传后的file文件
  * insertImgFn 将图片插入编辑器
  */
editor.config.customUploadImg = function (resultFiles, insertImgFn) {
  // 转base64
  getBase64(resultFiles, function(e){
    // 调用上传
    uploadImage(e).then(res => {
      // 上传成功 插入编辑器
      insertImgFn(res)
    })
  })
}
```


## 获取编辑器内容
```bash
editor.txt.html()
```

## 设置编辑器内容
```bash
let contentH = ``
editor.txt.html(contentH)
```

— END —
