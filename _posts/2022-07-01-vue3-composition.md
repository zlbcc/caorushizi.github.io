---
title: Vue.js 3.0 Composition APIs 及 3.0 原理剖析
author: Kaiser
date: 2022-07-01 19:34:00 +0800
categories: [Vue, CompositionApi]
tags: [大前端]
---

### Vue 3.0 性能提升主要是通过哪几方面体现的？

- 响应式系统升级：vue3 使用了 proxy 重写了响应式系统，因为 proxy 可以对整个对象进行监听，所以不需要对整个对象深度遍历
- 编译优化：vue3 中标记和提升所有的静态节点，diff 过程中只需要对比动态节点
- 源码体积优化：vue3 中移除了不经常使用的 API，tree-sharking 减小打包体积

### Vue 3.0 所采用的 Composition Api 与 Vue 2.x 使用的 Options Api 有什么区别？

Options Api: 包含一个描述组件选项（data,methods,props）的对象；开发复杂组件，同一个功能代码被拆分到不同选项中；
Composition Api： 一组基于函数的 API；可以灵活的组织组件的逻辑；更好的类型推导，容易集合 TypeScript；

### Proxy 相对于 Object.defineProperty 有哪些优点？

可以监听动态属性的添加；可以监听到数组的索引和数组 length 属性；可以监听删除属性；多层属性嵌套，在访问属性过程中处理下一级属性

### Vue 3.0 在编译方面有哪些优化？

vue2 中采用标记静态根节点，优化 diff 过程，来减少操作 dom；vue3 中标记和提升所有静态根节点,diff 过程只需要对比动态节点；Fragments；静态提升；Patch flag；缓存事件处理函数

### Vue.js 3.0 响应式系统的实现原理？

- reactive
  - 接受一个参数，typeof 判断这个参数是否是对象，不是对象直接返回这个参数，不能用 reactive 做响应式处理
  - 是对象，创建响应拦截器 handler,里面实现 get/set/deleteProperty
  - get：通过 track 收集依赖,通过 Reflect.get 获取当前 key 的值，注意这里如果 key 的值是个对象，要为该对象创建响应拦截器递归调用 reactive
  - set：先比较新旧 value 是否相等，相等直接返回,不想等，通过 Reflect.set 设置新的 value,并通过 trigger 触发更新，最后记得返回 boolean 值，否则会报 warn
  - deleteProperty:当前对象有这个 key 值，删除这个 key，并通过 trigger 触发更新，最后记得返回 boolean 值，否则会报 warn
- effect
  - 接受一个函数作为参数，记得把函数赋值给全局对象 activeEffect，作用是访问响应式对象属性时区收集依赖
- track
  - 接受两个参数 target 和 key
  - 如果没有 activeEffect，则说明没有创建 effect 依赖
  - 如果有 activeEffect，则去判断 WeakMap 集合中是否有 target 属性
    - WeakMap 集合中没有 target 属性，则 set(target, (depsMap = new Map()))
    - WeakMap 集合中有 target 属性，则判断 target 属性的 map 值的 depsMap 中是否有 key 属性
      - depsMap 中没有 key 属性，则 set(key, (dep = new Set()))
      - depsMap 中有 key 属性，则添加这个 activeEffect
- trigger
  - 判断 WeakMap 中是否有 target 属性
    - WeakMap 中没有 target 属性，则没有 target 相应的依赖
    - WeakMap 中有 target 属性，则判断 target 属性的 map 值中是否有 key 属性，有的话循环触发收集的 effect()
