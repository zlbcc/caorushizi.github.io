---
layout: post
title: "m3u8 视频下载（media downloader 使用说明）"
date: 2021-02-06 20:23:00 +0800
categories: ["我的开源软件"]
tags: ["electron", "m3u8", "流媒体下载", "m3u8 download"]
---

### 软件下载（暂时仅支持 windows ）

下载链接：[media-downloader-windows-v1.0.0](http://static.ziying.site/media-downloader-1.0.0%20Setup.exe)

源码地址：[github](https://github.com/caorushizi/media-downloader)

### 使用说明

1. 首先选择文件下载路径，配置完成之后再选择执行程序，提供两种选项

    - mediago: 源码地址[github](https://github.com/caorushizi/mediago)，使用go开发，支持windows和mac
    - N_m3u8DL-CLI: 源码地址[github](https://github.com/nilaoda/N_m3u8DL-CLI)，使用C#开发暂时仅支持windows
2. 填写视频名称和m3u8地址，可以使用打开浏览器，再页面地址栏中输入想要观看的视频，届时首页中会出现m3u8链接地址，点击可以带出请求地址、请求标头和视频名称。
   ![操作说明-1.png](http://static.ziying.site/博客图片/20210213011623.png)
   ![操作说明-2.png](http://static.ziying.site/博客图片/20210213011700.png)
3. 点击开始下载即可。
