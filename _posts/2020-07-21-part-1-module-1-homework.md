---
layout: post
title: "第一章模块一 函数式编程与JS异步编程、手写Promise"
date: 2020-7-21 22:42:00 +0800
categories: ["大前端"]
tags: ["泰康日记", "do homework"]
---

### 简答题

- 谈谈你是如何理解 JS 异步编程的，EventLoop、消息队列都是做什么的，什么是宏任务，什么是微任务？

javascript 为了实现浏览器页面的交互，不得不使用单线程的方式渲染页面，以保证 dom 可以被正确的渲染。然而单线程就以为着浏览器有多个任务的时候需要排队依次执行，这样就会造成阻塞。event loop 主线程从消息队列中读取事件，这个过程是一直重复循环的，所以整个机制被称作 event loop。消息队列是暂时存放异步任务的地方，我们的异步代码会存放到消息队列中，等到同步代码执行完毕以后，event loop 会从消息队列中依次取出异步任务放到调用栈中再次执行。宏任务可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。微任务可以理解是在当前 task 执行结束后立即执行的任务。也就是说，在当前 task 任务后，下一个 task 之前，在渲染之前。

### 代码题

#### 一、将下面的异步代码使用 Promise 的方式改进

```js
setTimeout(function () {
  var a = "hello";
  setTimeout(function () {
    var b = "lagou";
    setTimeout(function () {
      var c = "I ❤️ U";
      console.log(a + b + c);
    }, 10);
  }, 10);
}, 10);
```

```js
function genPromise(text) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(text);
    }, 10);
  });
}

(async () => {
  const a = await genPromise("hello");
  const b = await genPromise("lagou");
  const c = await genPromise("I ❤️ U");
  console.log(a + b + c);
})();
```

#### 二、基于以下代码完成下面的四个练习

```js
const fp = require("lodash/fp");
// 数据
// horsepower 马力， dollar_value 价格，in_stock 库存
const cars = [
  { name: "Ferrari FF", horsepower: 660, dollar_value: 700000, in_stock: true },
  {
    name: "Spyker C12 Zagato",
    horsepower: 650,
    dollar_value: 648000,
    in_stock: false,
  },
  {
    name: "Jaguar XKR-S",
    horsepower: 550,
    dollar_value: 132000,
    in_stock: false,
  },
  { name: "Audi R8", horsepower: 525, dollar_value: 114200, in_stock: false },
  {
    name: "Aston Martin One-77",
    horsepower: 750,
    dollar_value: 1850000,
    in_stock: true,
  },
  {
    name: "Pagani Huayra",
    horsepower: 700,
    dollar_value: 1300000,
    in_stock: false,
  },
];
```

- 练习 1：使用函数组合 fp.flowRight() 重新实现下面这个函数

```js
let isLastInStock = function (cars) {
  // 获取最后一条数据
  let last_car = fp.last(cars);
  // 获取最后一条数据的 in_stock 属性值
  return fp.prop("in_stock", last_car);
};
```

```js
const isLastInStock = fp.flowRight(fp.prop("in_stock"), fp.last);
console.log(isLastInStock(cars));
```

- 练习 2：使用 fp.flowRight()、fp.prop()和 fp.first()获取第一个 car 的 name

```js
const lastCarName = fp.flowRight(fp.prop("name"), fp.first);
console.log(lastCarName(cars));
```

- 练习 3：使用帮助函数 \_average 重构 averageDollarValue， 使用函数组合的方式实现

```js
let _average = function (xs) {
  return fp.reduce(fp.add, 0, xs) / xs.length;
}; // <- 无须改动
let averageDollarValue = function (cars) {
  let dollar_values = fp.map(function (car) {
    return car.dollar_value;
  }, cars);
  return _average(dollar_values);
};
```

```js
const dollar_value = fp.map(function (car) {
  return car.dollar_value;
});
const averageDollarValue = fp.flowRight(_average, dollar_value);
```

- 练习 4：使用 flowRight 写一个 sanitizeNames()函数，返回一个下划线连接的小写字符串，把数组中的 name 转换为这种形式：例如：sanitizeNames(["Hello World"]) => ['hello_world']

```js
let _underscore = fp.replace(/\W+/g, "_"); // <-- 无须改动，并在 sanitizeNames 中使用它
```

```js
const sanitizeNames = fp.map(fp.flowRight(_underscore, fp.toLower));
```

#### 三、基于下面提供的代码，完成后续的四个练习

```js
// support.js
class Container {
  static of(value) {
    return new Container(value);
  }

  constructor(value) {
    this._value = value;
  }

  map(fn) {
    return Container.of(fn(this._value));
  }
}

class Maybe {
  static of(x) {
    return new Maybe(x);
  }

  isNothing() {
    return this._value === null || this._value === undefined;
  }

  constructor(x) {
    this._value = x;
  }

  map(fn) {
    return this.isNothing() ? this : Maybe.of(fn(this._value));
  }
}

module.exports = { Maybe, Container };
```

- 练习 1：使用 fp.add(x,y) 和 fp.map(f, x)创建一个能让 functor 里的值增加的函数 ex1

```js
// app.js
const fp = require("lodash/fp");
const { Maybe, Container } = require("./support");
let maybe = Maybe.of([5, 6, 1]);
let ex1 = () => {
  // 你需要实现的函数。。。
};
```

```js
const addSome = fp.map(fp.add(1));
maybe.map(addSome).map(console.log);
```

- 练习 2：实现一个函数 ex2， 能够使用 fp.first 获取列表中的第一个元素

```js
// app.js
const fp = require("lodash/fp");
const { Maybe, Container } = require("./support");
let xs = Container.of(["do", "ray", "me", "fa", "so", "la", "ti", "do"]);
let ex2 = () => {
  // 你需要实现的函数。。。
};
```

```js
xs.map(fp.first).map(console.log);
```

- 练习 3：实现一个函数 ex3，使用 safeProp 和 fp.first 找到 user 的名字和首字母

```js
// app.js
const fp = require("lodash/fp");
const { Maybe, Container } = require("./support");
let safeProp = fp.curry(function (x, o) {
  return Maybe.of(o[x]);
});
let user = { id: 2, name: "Albert" };
let ex3 = () => {
  // 你需要实现的函数。。。
};
```

```js
safeProp("name", user).map(fp.first).map(console.log);
```

- 练习 4： 使用 Maybe 重写 ex4 ，不要有 if 语句

```js
// app.js
const fp = require("lodash/fp");
const { Maybe, Container } = require("./support");
let ex4 = function (n) {
  if (n) {
    return parseInt(n);
  }
};
```

```js
const { Maybe } = require("./support");
let ex4 = function (n) {
  Maybe.of(n).map(parseInt).map(console.log);
};
ex4(null);
```

#### 四、手写实现 MyPromise 源码

要求：尽可能还原 Promise 中的每一个 API，并通过注释的方式描述思路和原理

```js
const PENDING = "pending"; // pending
const FULFILLED = "fulfilled"; // fulfilled
const REJECTED = "rejected"; // rejected

export default class MyPromise {
  constructor(executor) {
    try {
      executor(this.resolve, this.reject);
    } catch (e) {
      this.reject(e);
    }
  }

  // promsie 状态
  status = PENDING;
  // 成功之后的值
  value = undefined;
  // 失败后的原因
  reason = undefined;
  // 成功回调
  successCallback = [];
  // 失败回调
  failCallback = [];

  resolve = (value) => {
    // 如果状态不是等待 阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为成功
    this.status = FULFILLED;
    // 保存成功之后的值
    this.value = value;
    // 判断成功回调是否存在 如果存在 调用
    while (this.successCallback.length) this.successCallback.shift()();
  };
  reject = (reason) => {
    // 如果状态不是等待 阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为失败
    this.status = REJECTED;
    // 保存失败后的原因
    this.reason = reason;
    // 判断失败回调是否存在 如果存在 调用
    // this.failCallback && this.failCallback(this.reason);
    while (this.failCallback.length) this.failCallback.shift()();
  };

  then(successCallback, failCallback) {
    // 参数可选
    successCallback = successCallback ? successCallback : (value) => value;
    // 参数可选
    failCallback = failCallback
      ? failCallback
      : (reason) => {
          throw reason;
        };
    let promsie2 = new MyPromise((resolve, reject) => {
      // 判断状态
      if (this.status === FULFILLED) {
        setTimeout(() => {
          try {
            let x = successCallback(this.value);
            // 判断 x 的值是普通值还是promise对象
            // 如果是普通值 直接调用resolve
            // 如果是promise对象 查看promsie对象返回的结果
            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
            resolvePromise(promsie2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        }, 0);
      } else if (this.status === REJECTED) {
        setTimeout(() => {
          try {
            let x = failCallback(this.reason);
            // 判断 x 的值是普通值还是promise对象
            // 如果是普通值 直接调用resolve
            // 如果是promise对象 查看promsie对象返回的结果
            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
            resolvePromise(promsie2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        }, 0);
      } else {
        // 等待
        // 将成功回调和失败回调存储起来
        this.successCallback.push(() => {
          setTimeout(() => {
            try {
              let x = successCallback(this.value);
              // 判断 x 的值是普通值还是promise对象
              // 如果是普通值 直接调用resolve
              // 如果是promise对象 查看promsie对象返回的结果
              // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
              resolvePromise(promsie2, x, resolve, reject);
            } catch (e) {
              reject(e);
            }
          }, 0);
        });
        this.failCallback.push(() => {
          setTimeout(() => {
            try {
              let x = failCallback(this.reason);
              // 判断 x 的值是普通值还是promise对象
              // 如果是普通值 直接调用resolve
              // 如果是promise对象 查看promsie对象返回的结果
              // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
              resolvePromise(promsie2, x, resolve, reject);
            } catch (e) {
              reject(e);
            }
          }, 0);
        });
      }
    });
    return promsie2;
  }

  finally(callback) {
    return this.then(
      (value) => {
        return MyPromise.resolve(callback()).then(() => value);
      },
      (reason) => {
        return MyPromise.resolve(callback()).then(() => {
          throw reason;
        });
      }
    );
  }

  catch(failCallback) {
    return this.then(undefined, failCallback);
  }

  static all(array) {
    let result = [];
    let index = 0;
    return new MyPromise((resolve, reject) => {
      function addData(key, value) {
        result[key] = value;
        index++;
        if (index === array.length) {
          resolve(result);
        }
      }

      for (let i = 0; i < array.length; i++) {
        let current = array[i];
        if (current instanceof MyPromise) {
          // promise 对象
          current.then(
            (value) => addData(i, value),
            (reason) => reject(reason)
          );
        } else {
          // 普通值
          addData(i, array[i]);
        }
      }
    });
  }

  static resolve(value) {
    if (value instanceof MyPromise) return value;
    return new MyPromise((resolve) => resolve(value));
  }
}

function resolvePromise(promsie2, x, resolve, reject) {
  if (promsie2 === x) {
    return reject(
      new TypeError("Chaining cycle detected for promise #<Promise>")
    );
  }
  if (x instanceof MyPromise) {
    // promise 对象
    // x.then(value => resolve(value), reason => reject(reason));
    x.then(resolve, reject);
  } else {
    // 普通值
    resolve(x);
  }
}
```
