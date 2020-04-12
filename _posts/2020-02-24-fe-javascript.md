---
layout: post
title: "javascript 前端面试"
date: 2020-3-24 16:20:00 +0800
categories: ["前端知识"]
tags: ["泰康日记", "前端"]
---

错误捕获
----

- 前端错误收集以及统一异常处理

来源： [https://juejin.im/post/5be2b0f6e51d4523161b92f0](https://juejin.im/post/5be2b0f6e51d4523161b92f0 "掘金")

1. 客户端收集

window.onerror: 会全局的在JavaScript运行时错误、语法错误发生时触发。

```javascript
window.onerror = (msg, url, lineNum, colNum, err) => {
  console.log(`错误发生的异常信息（字符串）:${msg}`);
  console.log(`错误发生的脚本URL（字符串）:${url}`);
  console.log(`错误发生的行号（数字）:${lineNum}`);
  console.log(`错误发生的列号（数字）:${colNum}`);
  console.log(`错误发生的Error对象（错误对象）:${err}`)
};
```