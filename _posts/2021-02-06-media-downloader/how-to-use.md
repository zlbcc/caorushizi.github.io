---
layout: post
title: "m3u8 视频下载（media downloader 使用说明）"
date: 2021-02-06 20:23:00 +0800
categories: ["我的开源软件"]
tags: ["Electron"]
---

### 软件下载（暂时仅支持 windows ）

下载链接：[media-downloader-windows-v0.0.1](http://static.ziying.site/media-downloader-1.0.0%20Setup.exe)

源码地址：[github](https://github.com/caorushizi/media-downloader)

### 使用说明

首先需要参照`使用说明`配置相关环境变量

1. 首先选择文件下载路径，配置完成之后再选择执行程序，提供两种选项

    - mediago: 源码地址[github](https://github.com/caorushizi/mediago)，使用go开发，支持windows和mac
    - N_m3u8DL-CLI: 源码地址[github](https://github.com/nilaoda/N_m3u8DL-CLI)，使用C#开发暂时仅支持windows
2. 填写视频名称和m3u8地址，可以使用打开浏览器，再页面地址栏中输入想要观看的视频，届时首页中会出现m3u8链接地址，点击可以带出请求地址、请求标头和视频名称。
   ![操作说明-1.png](http://static.ziying.site/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/%E6%93%8D%E4%BD%9C%E8%AF%B4%E6%98%8E-1.png)
   ![操作说明-2.png](http://static.ziying.site/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/%E6%93%8D%E4%BD%9C%E8%AF%B4%E6%98%8E-2.png)
3. 点击开始下载即可。

### 环境配置

在开始使用之前需要先配置电脑的环境变量

1. 我们可以在 D 盘根目录下建立Envs文件夹，在Envs文件夹下创建bin目录将bin目录加入环境变量中
2. 下载可执行程序 `ffmpeg` 、 `N_m3u8DL-CLI` 、 `mediago`
   - ffmpeg: [下载链接](https://www.gyan.dev/ffmpeg/builds/)
   - mediago: [下载链接](http://static.ziying.site/mediago.exe)
   - N_m3u8DL-CLI: [下载链接](https://github.com/nilaoda/N_m3u8DL-CLI/releases/tag/2.9.4) 下载完成后需要将可执行程序的名称设置为 N_m3u8DL-CLI_v2.9.4.exe ,如下图
     
     ![操作说明-3.png](http://static.ziying.site/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/%E6%93%8D%E4%BD%9C%E8%AF%B4%E6%98%8E-3.png)



