---
title: ionic 相机拍照上传与相册上传
date: 2020-04-13
tags: ['ionic']
---

# 准备工作

安装两个`cordova`插件

相机插件
```bash
ionic cordova plugin add cordova-plugin-camera
npm install @ionic-native/camera
```
相册插件
```bash
ionic cordova plugin add cordova-plugin-telerik-imagepicker
npm install @ionic-native/image-picker
```

# 关键代码
```bash
import { Injectable } from '@angular/core'
import { Camera } from "@ionic-native/camera" // 相机插件
import { ImagePicker } from "@ionic-native/image-picker" // 相册上传插件
import { UtilService } from '../service/util'

@Injectable()
export class UploadService {
  // 相机调用所需参数 更多参数自行阅读api
  cameraOptions: any = {
    quality: 100, // 图像质量范围为0-100。默认50
    destinationType: 0, // 默认0（DATA_URL），返回base64代码
    sourceType: 1, // 0-图片库 图片来源 1-摄像头 2-保存相册 后来发现0是图片库可能也能调起相册，但我还没尝试。
    encodingType: 0, // 选择返回的图像文件的编码 0-JPEG 1PNG
    mediaType: 0, // 媒体类型 0-图片 1-视频 2-所有
    allowEdit: true, // 允许简单的编辑图像
    correctOrientation: true // 允许旋转图像以校正捕获时设备的方向
  }
  // 相册调用所需参数
  imagePrickOptions: any = {
    maximumImagesCount: 1, //选择一张图片 默认15张
    quality: 100, // 图像质量范围为0-100。默认50
    outputType: 1 // 返回base64
  }
  constructor(
    private util: UtilService
  ) {}

  // 打开相机
  openCamera (quality) {
    this.cameraOptions.quality = quality
    let _this = this
    return new Promise(function(resolve) {
      Camera.getPicture(_this.cameraOptions).then((imageData) => {
        // 返回base64编码 注意: 这里的base64默认不带 `data:image/jpeg;base64,`，需要我们手动添加
        resolve('data:image/jpeg;base64,' + imageData)
      }, (err) => {
        _this.util.toast('摄像头打开失败', 'error')
      })
    })
  }

  // 打开手机相册
  openImgPicker () {
    let _this = this
    return new Promise(function(resolve) {
      ImagePicker.getPictures(_this.imagePrickOptions).then((results) => {
        // 返回base64
        resolve('data:image/jpeg;base64,' + results)
      }, (err) => {
        _this.util.toast('相册打开失败', 'error')
      })
    })
  }
}

```

# 使用

操作控件
html
```bash
<div class="upload_box" (click)="uploadSheet()">
  <i class="icos iconfont">&#xe613;</i>
  <p>点击拍照或上传</p>
</div>
```
ts
```bash
async uploadSheet() {
  const actionSheet = await this.actionSheetController.create({
    header: '请选择',
    buttons: [
      {
        text: '拍照',
        handler: () => {
          this.upload.startCamera(100).then((result) => {
            // result: 图片base64
            if (result) {
              // 处理编码上传服务器
              ...
              // 我这里是转为blob
            }
          })
        }
      },
      {
        text: '从相册选取',
        handler: () => {
          // result: 图片base64
          this.upload.openImgPicker().then((result) => {
            if (result) {
              // 处理编码上传服务器
              ...
            }
          })
        }
      },
      {
        text: '取消',
        role: 'cancel'
      }
    ]
  })
  await actionSheet.present()
}
```

— END —
