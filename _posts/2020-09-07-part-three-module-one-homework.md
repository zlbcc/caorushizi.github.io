---
layout: post
title: "第三章模块一 手写 Vue Router、手写响应式实现、虚拟 DOM 和 Diff 算法"
date: 2020-09-07 12:18:00 +0800
categories: ["大前端"]
tags: ["泰康日记", "do homework"]
---

### 一、简答题

#### 当我们点击按钮的时候动态给 data 增加的成员是否是响应式数据，如果不是的话，如何把新增成员设置成响应式数据，它的内部原理是什么。

```javascript
let vm = new Vue({
  el: '#el',
  data: {
    o: 'object',
    dog: {}
  },
  method: {
    clickHandler () {
      // 该 name 属性是否是响应式的
      this.dog.name = 'Trump'
    }
  }
})
```

1. 不是响应式的，对于已经创建的实例，Vue不允许动态添加根级别的响应式属性
2. 使用Vue.set(vm.dog, 'name', 'dog_name')或this.$set(this.dog, 'name', 'dog_name')
3. this.$set在new Vue()时候就被注入到Vue的原型上，set方法内部仍是调用了defineReactive()方法进行响应式处理

##### 请简述 Diff 算法的执行过程

1. diff的过程就是调用名为patch(el, vnode)/patch(oldVnode, vnode)的函数，比较新旧节点，一边比较一边给真实DOM打补丁
2. patch里会调用sameVnode(oldVnode, vonde)，根据返回结果：
    - true: 则执行patchVnode
    - false: 则用vnode替换oldVnode
3. patchVnode(oldNode, vnode, insertedVnodeQueue)
    - 找到对应的真实DOM，成为el
    - 判断vnode和oldVnode是否指向同一个对象，如果是，直接return
    - 如果它们都有文本节点并且不相等，那么将el的文本节点设置为vnode的文本文本节点
    - 如果oldVnode有子节点而vnode没有，则删除el的子节点
    - 如果oldVnode没有子节点而vnode有，则将vnode的子节点真实化之后加到el
    - 如果两者都有子节点，则执行updateChildren(parentElm, oldCh, newCh)函数比较子节点(key很重要)

### 二、编程题

#### 模拟 VueRouter 的 hash 模式的实现，实现思路和 History 模式类似，把 URL 中的 # 后面的内容作为路由的地址，可以通过 hashchange 事件监听路由地址的变化。

#### 在模拟 Vue.js 响应式源码的基础上实现 v-html 指令，以及 v-on 指令。

#### 参考 Snabbdom 提供的电影列表的示例，利用Snabbdom 实现类似的效果，如图：

![figure1](http://static.ziying.site/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/Ciqc1F7zUZ-AWP5NAAN0Z_t_hDY449.jpg)

