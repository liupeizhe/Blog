---
title: 微信小程序（一）-搭建小程序
date: 2020-01-12
tags: ['微信小程序']
---

工具：*微信开发者工具*、*Visual Studio Code*

下面开始搭建

一、下载 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，傻瓜式安装。

二、进入 [微信公众平台](https://mp.weixin.qq.com/)，登录后找到小程序的AppId，复制下来。

![查看AppId](/images/wechat-applets/1.jpg)

三、打开安装好的微信开发者工具，登录后选择小程序项目。

![小程序项目](/images/wechat-applets/2.jpg)

四、 将AppId复制到AppId输入框里，选择项目构建目录，点击确定。

![构建项目](/images/wechat-applets/3.jpg)

五、出现如下图所示的界面，微信开发工具已经帮我们搭建了一个简单的项目了，到此你的第一个项目就搭建成功了~

![搭建成功](/images/wechat-applets/4.jpg)

六、下面来看一下项目的目录结构和文件

1. pages主要存放文件信息
   它包含4个文件：js（必须），wxml（必须），json（非必须），wxss（非必须）
   .js用来处理业务逻辑。
   .wxml相当于html，用来定义页面内容。
   .json用来页面配置，title、背景色等。
   .wxss相当于css，用来写样式。

2. `app.js`是公共的js文件，主要处理一些全局的小程序逻辑。例如进入小程序后判断用户登录状态就可以在这里做。

```bash
//app.js
App({
  onLaunch: function () {
    // 展示本地存储能力
    var logs = wx.getStorageSync('logs') || []
    logs.unshift(Date.now())
    wx.setStorageSync('logs', logs)

    // 登录
    wx.login({
      success: res => {
        // 发送 res.code 到后台换取 openId, sessionKey, unionId
      }
    })
    // 获取用户信息
    wx.getSetting({
      success: res => {
        if (res.authSetting['scope.userInfo']) {
          // 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
          wx.getUserInfo({
            success: res => {
              // 可以将 res 发送给后台解码出 unionId
              this.globalData.userInfo = res.userInfo

              // 由于 getUserInfo 是网络请求，可能会在 Page.onLoad 之后才返回
              // 所以此处加入 callback 以防止这种情况
              if (this.userInfoReadyCallback) {
                this.userInfoReadyCallback(res)
              }
            }
          })
        }
      }
    })
  },
  globalData: {
    userInfo: null
  }
})
```

3. `app.json`该文件中存放的公共配置，json格式，其中pages是必须要配置的，程序中的每一个页面，都需要在这里配置，否则页面会找不到。
   进入小程序后，小程序找的第一个页面就是pages中配置的第一个页面，所以可以把小程序启动页放在第一个。
   window属性配置的是一些窗口属性。

```bash
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  }
}
```

4. `app.wxss`是小程序的公共样式表。

```bash
/**app.wxss**/
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 200rpx 0;
  box-sizing: border-box;
} 
```

5. `project.config.json`是整个项目的环境配置文件。

```bash
{
  "description": "项目配置文件",
  "packOptions": {
    "ignore": []
  },
  "setting": {
    "urlCheck": true,
    "es6": true,
    "postcss": true,
    "minified": true,
    "newFeature": true
  },
  "compileType": "miniprogram",
  "libVersion": "2.2.5",
  "appid": "wx3639f434d0dce6cc",
  "projectname": "helloworld",
  "debugOptions": {
    "hidedInDevtools": []
  },
  "isGameTourist": false,
  "condition": {
    "search": {
      "current": -1,
      "list": []
    },
    "conversation": {
      "current": -1,
      "list": []
    },
    "game": {
      "currentL": -1,
      "list": []
    },
    "miniprogram": {
      "current": -1,
      "list": []
    }
  }
}
```

— END —
