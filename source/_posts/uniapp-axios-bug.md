---
title: uniapp-axios真机无法发送请求问题
date: 2020-11-18
tags: ['uniapp']
---

我的项目中没有使用官方提供的`uni.request`，而是使用了`axios`，但遇到了一个问题是在我本地调试没问题，但真机运行时无法发送请求，会提示`adapter is not a function`。

解决方案：配置axios适配器（axios-adapter-uniapp）。

```bash
Using npm:

$ npm install axios-adapter-uniapp

Using yarn:

$ yarn add axios-adapter-uniapp
```

[axios-adapter-uniapp传送门](https://www.npmjs.com/package/axios-adapter-uniapp)

使用：
```bash
import axios from 'axios'
import axiosAdapterUniapp from 'axios-adapter-uniapp'
 
const instance = axios.create({
  baseURL: 'URL.com',
  adapter: axiosAdapterUniapp
})
```

贴一下我的拦截器：

```bash
import axios from 'axios'
import Vue from 'vue'
import axiosAdapterUniapp from 'axios-adapter-uniapp'
import store from 'store/index.js'

const baseURL = 'https://xxxxx'
Vue.prototype.$baseUrl = baseURL

var instance = axios.create({
	baseURL,
	timeout: 15000,
	withCredentials: true,
	crossDomain: true,
	adapter: axiosAdapterUniapp // 适配器
})

instance.interceptors.request.use(
  request => {
	var token = store.getters.token
	if (token) {
		request.headers['Token'] = token
	}
    return Promise.resolve(request)
  }
)

// response interceptor
instance.interceptors.response.use(
  response => {
		var res = response.data
    if (response.status === 200 && res.code < 0) {
			// 失败
			if (res.code === -1 || res.code === -4) {
				uni.showToast({
					title: res.message,
					icon: 'none'
				})
			}
      // 未登录处理
      if (res.code === -2) {
        ...
      }
			// 无权限
			if (res.code === -5) {
        ...
      }
      return Promise.reject(new Error(res.message || 'Error'))
    }
    // 请求成功
		if (response.status === 200 && res.code >= 0) {
      // 一个小坑，真机返回的header里字段是`Token`，本地调试的字段是`token`
			var respToken = response.header['Token'] || response.header['token']
			if (respToken) {
        // 将token存储至VUEX
				store.dispatch('user/setToken', respToken)
			}
      return res
    }
  },
  error => {
    console.log('Err:' + error) // for debug
    return Promise.reject(error)
  }
)

export default instance

```

— END —
