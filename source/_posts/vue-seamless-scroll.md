---
title: Vue无缝滚动插件-vue-seamless-scroll
date: 2021-03-20 10:56:28
tags: ['vue']
---

我的需求是实现从下往上滚动，所以本文也只实现这一个，其余效果请见[官方文档]('https://chenxuan1993.gitee.io/component-document/index_prod#/component/seamless-default')

# 准备工作

1. 安装`vue-seamless-scroll`依赖
```bash
$ npm install vue-seamless-scroll
$ yarn add vue-seamless-scroll
```

2. 在`main.js`导入并引用
```bash
import scroll from 'vue-seamless-scroll' // 滚动插件

Vue.use(scroll)
```

# 实现从上往下滚动

**注意：需要给父容器一个height和:data='Array'和overfolw:hidden;左右滚动需要给ul容器一个初始化 css width。**

+ HTML部分
```bash
<div class="scrollBox">
  <vue-seamless-scroll :data="fastList" class="seamless-warp">
    <li v-for="(item, index) in fastList" :key="index">
      <div class="urgentBox">
        <p class="productTitle">
          <span class="fontStyle">{{item.releaseName}}</span>
          <span :class="item.releaseType === '收购' ? 'acquisition' : 'sell'">{{item.releaseType}}</span>
          <span class="fontStyle">
            {{item.price}}元/公斤
          </span>
        </p>
        <div class="productInfo">
          <p class="fontStyle">
            <span class="spot spot2">现货</span>
            {{item.remarks}}
          </p>
          <p class="fontStyle">需求量：{{item.totleNum}}斤</p>
          <div class="message">
            <span @click="openDialog(item)">留言</span>
          </div>
        </div>
      </div>
    </li>
  </vue-seamless-scroll>
</div>
```

+ JS部分
```bash
<script>
export default {
  name: 'fast',
  data() {
    return {
      fastList: [...] // 列表
    }
  }
}
</script>
```

+ CSS部分
```bash
.scrollBox
  max-height 1500px
  padding 0px
  overflow-y hidden !important
  border 1px solid #f5f5f5ab
.seamless-warp
  overflow hidden
  >>> div
    overflow unset
    height 100%
ul, li
  list-style-type none
```

— END —
