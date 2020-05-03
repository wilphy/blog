> #### JavaScript 基本数据类型

> #### 原型与原型链

> #### Javascript 如何实现继承？

> #### Javascript 创建对象的几种方式？

> #### new 运算符的执行过程

1. 新生成一个对象
2. 链接到原型: obj.**proto** = Con.prototype
3. 绑定 this: apply
4. 返回新对象(如果构造函数有自己 retrun 时，则返回该值)

> #### 作用域

> #### 闭包

> #### 异步与单线程

> #### 手写 Promise

> #### Promise.all

> #### Promise.race

> #### async / await

> #### documen.write 和 innerHTML 的区别?

> #### 手写函数防抖和节流

防抖与节流函数是一种最常用的 高频触发优化方式，能对性能有较大的帮助。

- `防抖 (debounce)`: 将多次高频操作优化为只在最后一次执行，通常使用的场景是：用户输入，只需再输入完成后做一次输入校验即可。

  ```js
  function debounce(fn, wait, immediate) {
    let timer = null;

    return function () {
      let args = arguments;
      let context = this;

      if (immediate && !timer) {
        fn.apply(context, args);
      }

      if (timer) clearTimeout(timer);
      timer = setTimeout(() => {
        fn.apply(context, args);
      }, wait);
    };
  }
  ```

- 节流(throttle): 每隔一段时间后执行一次，也就是降低频率，将高频操作优化成低频操作，通常使用场景: 滚动条事件 或者 resize 事件，通常每隔 100~500 ms 执行一次即可。

  ```js
  function throttle(fn, wait, immediate) {
    let timer = null;
    let callNow = immediate;

    return function () {
      let context = this,
        args = arguments;

      if (callNow) {
        fn.apply(context, args);
        callNow = false;
      }

      if (!timer) {
        timer = setTimeout(() => {
          fn.apply(context, args);
          timer = null;
        }, wait);
      }
    };
  }
  ```

> #### 手写 Ajax

> #### this

> #### call / bind / apply

> #### 浅拷贝/深拷贝

> #### map / filter / reduce / forEach

> #### 数组去重

> #### 正则

> #### let / var / const

