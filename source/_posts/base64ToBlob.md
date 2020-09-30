---
title: 图片base64编码转Blob
date: 2020-04-15
tags: ['前端']
---

```bash
upload () {
  let baseUrl = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABDgAAAhwCAYA.......'
  this.base64ToBlob(baseUrl)
  let blob = this.upload.base64ToBlob(baseUrl),
    formData = new FormData()
  // 组装formData传至服务器  
  formData.append('img', blob, 'xxx.png')
  ...
}
```

```bash
// base64转blob
base64ToBlob (baseUrl) {
  let arr = baseUrl.split(',')
  let mime = arr[0].match(/:(.*?);/)[1],
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n)
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }
  return new Blob([u8arr], { type: mime })
}
```

— END —
