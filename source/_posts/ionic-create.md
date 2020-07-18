---
title: ionic + Angular环境搭建
date: 2020-01-05
tags: ['ionic', '前端']
---

本机操作系统：*Windows 10*

## 安装node & npm

访问node.js官网，下载node，node与npm捆绑安装的，无需单独安装。
安装后查看node、npm版本，终端输入：

``` bash
node -v(-version)
npm -v(-version)
```

输出版本号代表安装成功。

## 安装 ionic CLI

终端输入：

``` bash
(sudo) npm install -g ionic
```

验证版本号：

``` bash
ionic -v
```

## 安装Cordova

终端输入：

``` bash
(sudo) npm install -g cordova
```

验证版本号：

``` bash
cordova -v
```

## 创建项目

有三种最常见的启动器可供选择，分别是`blank`启动器，`tabs`启动器和`sidemenu`启动器。

终端输入：

``` bash
(sudo) npm install -g cordova
```

等待创建完成。

## 启动项目

打开终端进入项目所在路径，输入：

``` bash
cd myApp
ionic serve
```

启动成功后会自动打开浏览器。
默认端口：8100

— END —
