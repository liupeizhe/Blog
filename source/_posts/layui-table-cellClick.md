---
title: layui-单元格自定义元素点击事件
date: 2020-11-30
tags: ['layui']
---

页面如下：

![table](/images/layui/01.jpg)

需要做到点击操作里的两个按钮，拿到该条数据，并做一些操作，因为此一次使用layui，着实费了点力气，过程中一度让我怀念vue...

代码如下：
```bash
{ field: 'addrId', title: '收货地址', },
{
  fixed: 'right',
  title: '操作',
  width: 200,
  templet(d) {
    if (d.state === 0) {
      return `<span class="setUp" data-id=${d.id} data-type=${1}>设为已处理</span><span class="setUp" data-id=${d.id} data-type=${2}>设为缺货</span>`
    }
    if (d.state === 2) {
      return `<span class="setUp" data-id=${d.id} data-type=${1}>设为已处理</span><span class="setUp" data-id=${d.id} data-type=${0}>设为待确认</span>`
    }
    if (d.state === 1) {
      return ``
    }
  }
}
```

生成表格的时候，我们使用`data-xxx="${d.xxx}"`来自定义数据，动态赋值，比如id。

然后：
```bash
$(document).on('click',' .setUp',function() {
  var id = $(this).data('id')
  var type = $(this).data('type')
  var reqParams = [
    { id, type }
  ]
  // 其他操作
  ...
})
```

我们监听class点击事件，使用`$(this).data('xxxx')`获取自定义的数据，再拿数据进行其他操作就好了。

— END —
