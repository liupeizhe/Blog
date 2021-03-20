---
title: vue-video-player 实现广告轮播和播放直播
date: 2021-03-20 09:57:21
tags: ['vue', '腾讯云直播']
---

# 准备工作

1. 安装`vue-video-player`依赖
```bash
$ npm install vue-video-player
$ yarn add vue-video-player
```

2. 引用
 2.1 全局引用，在`main.js`导入并引用
  ```bash
  import VideoPlayer from 'vue-video-player'
  import 'vue-video-player/src/custom-theme.css'
  import 'video.js/dist/video-js.css'

  Vue.use(VideoPlayer)
  ```
 2.2 组件内引用
  ```bash
  import { videoPlayer } from 'vue-video-player'
  import 'video.js/dist/video-js.css'

  export default {
    components: {
      videoPlayer
    }
  }
  ```

# 广告轮播

+ HTML部分
```bash
<video-player class="video-player vjs-custom-skin" 
  ref="videoPlayer" 
  :playsinline="true" 
  :options="playerOptions"
  @ended="onPlayerEnded">
</video-player>
```

+ JS部分
```bash
<script>
export default {
  name: 'videoDemo',
  data() {
    return {
      flag: 0, // 记录播放位置
      videos: [], // 视频列表
      // 播放器相关配置
      playerOptions: {
        playbackRates: [0.5, 1.0, 1.5, 2.0], // 可选的播放速度
        autoplay: true, // 如果为true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 是否视频一结束就重新开始。
        preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: 'zh-CN',
        aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [{ type: 'video/mp4', src:  '' }], // 因为我们要动态加载，所以src默认为空
        poster: '', // 封面地址
        notSupportedMessage: '此视频暂无法播放，请稍后再试', // 允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true, // 当前时间和持续时间的分隔符
          durationDisplay: true, // 显示持续时间
          remainingTimeDisplay: false, // 是否显示剩余时间功能
          fullscreenToggle: true // 是否显示全屏按钮
        }
      }
    }
  },
  mounted () {
    this.getVideos()
  },
  methods: {
    // 获取视频列表
    getVideos () {
      this.videos = ['/static/video/video1.mp4', '/static/video/video2.mp4', '...'] // 从服务器获取视频列表
      this.playerOptions['sources'][0]['src'] = this.videos[0] // 首先加载第一个
    },

    // 视频播完回调
    onPlayerEnded() {
      // 下标 +1
      this.flag += 1
      // 放完从第一个播放 否则直接播放下一个
      if (this.flag + 1 > this.videos.length) {
        this.flag = 0
        this.playerOptions['sources'][0]['src'] = this.videos[0]
      } else {
        this.playerOptions['sources'][0]['src'] = this.videos[this.flag]
      }
    }
  }
}
</script>
```

+ CSS部分
```bash
.video-player
  width 100% !important
  height 100%
  background-color rgb(0, 0, 0)
  >>> .video-js
    height 100%
    padding 0
  >>> video
    object-fit fill !important // 解决黑边问题 将视频充满盒子
  // 隐藏屏幕中央播放按钮和下方进度条 按需添加
  >>> .vjs-big-play-button, >>> .vjs-control-bar
    display none !important
```

# 播放腾讯云直播

获取直播地址，将`source`替换掉即可。
```bash
this.playerOptions['sources'][0]['src'] = 'http://xxxx/xxx.m3u8''
```

— END —
