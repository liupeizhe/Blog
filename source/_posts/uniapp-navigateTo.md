---
title: uniapp-自定义uni.navigateTo
date: 2020-11-15
tags: ['uniapp']
---

官方提供跳转方法就是：

```bash
uni.navigateTo({
  url: 'xxx?name=xx&age=xx...'
})
```

如果需要传递多个参数，`?`后面就得跟很长一串，看起来很乱，所以我们把它改成使用json格式传参。

创建文件`navTo.js`

代码：
```bash
/**
 * @export 
 * @param {*} url 跳转地址
 * @param {*} data 参数 json格式
 * @returns
 */
const go = function(url, data){
	url += (url.indexOf('?') < 0 ? '?' : '&') + param(data)
	
	uni.navigateTo({  
		url
	})
}

export function param (data) {
  let url = ''
  for (var k in data) {
    let value = data[k] !== undefined ? data[k] : ''
    url += '&' + k + '=' + encodeURIComponent(value)
  }
  return url ? url.substring(1) : ''
}
 
export {go}

```

然后再`main.js`中注册一下：
```bash
import * as navTo from '@/utils/navTo.js'
...
Vue.prototype.$navTo = navTo
```

使用：
```bash
this.$navTo.go('xxx', {
  name: 'xx',
  age: 'xx',
  ...
})
```

— END —
