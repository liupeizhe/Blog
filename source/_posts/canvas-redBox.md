---
title: canvas初体验-展示base64图片&画方框
date: 2020-04-28
tags: ['canvas', '前端']
---

需求：手机拍车牌号照片传值后端，后端返回车牌号信息以及车牌号图片位置，前端用方框框出来。

先看一下后端返回格式：
```bash
[{
  rect: [11, 22, 33, 44], // x1, y1, x2, y2
  p: 0.876543, // 车牌准确率
  license: '鲁A12345' // 识别内容
}]
```
上代码。

html

```bash
<div class="canvasStyle">
  <canvas id="myCanvas" height="300" height="300"></canvas>
</div>
```

css

```bash
.canvasStyle {
  text-align: center;
  #myCanvas {
    width: 100%;
  }
}
```

ts

```bash
/**
* @params
* array 后端返回数据
* base64Img 图片代码
*/
success (array, base64Img) {
  if (!array.length) {
    this.util.toast('未识别到车牌', 'warning')
  }
  // 绘制出图片与方框
  let canvas = document.getElementById('myCanvas') as HTMLCanvasElement
  // 设置canvas宽高
  canvas.width = document.body.clientWidth
  canvas.height = document.body.clientHeight
  let ctx: CanvasRenderingContext2D = canvas.getContext('2d'),
    img = new Image(),
    imgW = 0,
    imgH = 0,
    rectArray = []
  img.src = base64Img
  img.onload = function() {
    imgW = img.width
    imgH = img.height
    // 图片超过屏幕宽度则满屏 否则自适应
    if ((imgH * canvas.width / imgW) > canvas.height) {
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
    } else {
      ctx.drawImage(img, 0, 0, canvas.width, imgH * canvas.width / imgW)
    }
    // 有识别结果 画框
    if (array.length) {
      // 计算展示图与原图缩小比例
      let per = parseFloat((canvas.offsetWidth / imgW).toFixed(2))
      array.forEach((element, index) => {
        ctx.strokeStyle = '#ff0000' // 框颜色
        ctx.lineWidth = 2 // 框宽度
        let [x1, y1, x2, y2] = element.rect
        // 开始画框
        ctx.strokeRect(x1 * per, y1 * per, (x2 - x1) * per, (y2 - y1) * per)
        
        // 记录下几个框的信息 点击时要用
        rectArray.push({ x: x1 * per, y: y1 * per, width: (x2 - x1) * per, height: (y2 - y1) * per, plateNum: element.license })
      })
      // 画布点击事件
      canvas.addEventListener('click', function(e) {
        let p = getEventPosition(e)
        draw(p)
      }, false)
    }
    let _this = this
    function getEventPosition(ev) {
      if (ev.layerX || ev.layerX === 0) {
        return { x: ev.layerX, y: ev.layerY }
      } else if (ev.offsetX || ev.offsetX === 0) { // Opera
        return { x: ev.offsetX, y: ev.offsetY }
      }
    }
    function draw(p) {
      rectArray.forEach(function(v) {
        ctx.beginPath()
        ctx.rect(v.x, v.y, v.width, v.height)
        // 点击不同车牌重新获取数据
        if(p && ctx.isPointInPath(p.x, p.y)) {
          _this.openMode(v.plateNum)
        }
      })
    }
  }, 500)
}

```

— END —
