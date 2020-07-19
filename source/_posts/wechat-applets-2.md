---
title: 微信小程序（二）-请求接口
date: 2020-01-15
tags: ['微信小程序']
---

官方给出的方式是`wx.request`，请求方式比较简单，下面是官方给出的请求实例。

```bash
wx.request({
  url: 'test.php', //仅为示例，并非真实的接口地址
  data: {
    x: '' ,
    y: ''
  },
  header: {
    'content-type': 'application/json'
  },
  success: function(res) {
    console.log(res.data)
  }
})
```

+ url：请求的接口地址
+ data：请求参数
+ header：请求头，包括host、User-Agent、Accept、Accpt-Language、Referer等
+ method：请求方式：POST/GET
+ success：接口请求成功回调，获取从服务器返回的数据。请求成功后获取到的数据就是success函数的参数result。
+ fail：接口请求失败回调。
+ complete：接口调用结束回调(调用成功、失败都会执行),通过this.setData将数据传递给WXML页面。

可以写一个公共函数，所有用到请求的地方都调用着一个函数就好了，下面是我在项目中用到的。

*util.js*

```bash
/**
 * 发送HTTPS请求
 * @param
 * method 请求方式
 * url 接口地址
 * data 参数
 */
function sendRequest (method, url, data) {
  let _this = this
  let header = { 'content-type': 'application/json' }
  # cookie携带其他参数，视情况而定。
  let cookie = wx.getStorageSync('cookieKey')
  if (cookie) {
    header.Cookie = cookie
  }
  # Promise是异步编程的一种解决方案
  let pro = new Promise(function(resolve) {
    wx.request({
      url: '服务器地址' + url,
      method,
      header,
      data,
      success (res) {
        # 成功后的处理
        ...
        resolve(res)
      }
    })
  })
  # 告诉调用函数已经请求完成
  return pro
}

# 因为上面这段代码不是写在app.js 所以需要暴露出去
module.exports = {
  sendRequest
}

# 在app.js引入utils.js，如果不写这一句别的地方调用不到。
# 如果你将代码写在app.js中的话就不用管这些了。
utils: require('utils.js'),
```

调用方式：
例如在login.js中

```bash
# 获取应用实例
const app = getApp()

doLogin () {
  app.utils.sendRequest('POST', '接口地址', {
    username: 'xxx',
    password: 'xxx'
  }).then(res => {
    # 成功返回后处理数据
    if (res.data.code) {
      wx.showToast({ title: '登录成功', icon: 'success', duration: 1000 })
      # 坑爹的setData...
      # 这里注意，返回数据不能直接用 this.xxx = xxx 来赋值，不仅不会作用到页面，还会导致数据不一致，所以切记，使用`this.setData({})`来做页面回显。
      this.setData({
        userInfo: res.userInfo
      })
      ...
    }
  })
}
```

— END —
