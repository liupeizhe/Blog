---
title: el-upload通过下标删除图片
date: 2020-01-24
tags: ['vue', 'Element-Ui']
---

官方文档并没有提供按照下标删除图片池中图片的方法，所以只好自己想办法实现了。
本方法适用于上传图片后，服务器返回图片id，最后跟随表单统一提交的情况，如果只是正常上传按照文档做就好了。

上代码：

```bash
# 定义组件
<el-upload
  action
  list-type="picture-card"
  :on-remove="handleRemove"
  :http-request="uploadImg">
  <i class="el-icon-plus"></i>
</el-upload>
```

上传至服务器：

```bash
# 上传图片
uploadImg (fileObj) {
  console.log(fileObj)
  # 组装FormData
  let formData = new FormData()
  formData.append('file', fileObj.file)
  let config = {
    headers: { 'Content-Type': 'multipart/form-data' }
  }
  upload(formData, config).then(res => {
    if (res.data.success) {
      let imgId = res.data.data
      # 保存下id
      this.imageIds.push(imgId)
      # 保存下本次上传文件的唯一uid
      this.fileList.push(fileObj.file.uid)
    }
  })
}
```

删除图片：

```bash
handleRemove (file, fileList) {
  # 根据当前要删除图片uid，获取该uid在uid数组中所对应的下标
  const index = this.fileList.findIndex(item => item === file.uid)
  # 在imageIds数组中找到它！干掉他！
  this.imageIds.splice(index, 1)
},
```

— END —
