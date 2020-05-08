> #### 你能讲一讲 MVVM 吗？

MVVM 是 Model-View-ViewModel 缩写，也就是把 MVC 中的 Controller 演变成 ViewModel。Model 层代表数据模型，View 代表 UI 组件，ViewModel 是 View 和 Model 层的桥梁，数据会绑定到 viewModel 层并自动将数据渲染到页面中，视图变化的时候会通知 viewModel 层更新数据。

> #### 简单说一下 Vue2.x 响应式数据原理

Vue 在初始化数据时，会使用 Object.defineProperty 重新定义 data 中的所有属性，当页面使用对应属性时，首先会进行依赖收集(收集当前组件的 watcher)如果属性发生变化会通知相关依赖进行更新操作(发布订阅)。

> #### 那你知道 Vue3.x 响应式数据原理吗？

Vue3.x 改用 Proxy 替代 Object.defineProperty。因为 Proxy 可以直接监听对象和数组的变化，并且有多达 13 种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

- Proxy 只会代理对象的第一层，那么 Vue3 又是怎样处理这个问题的呢？

  判断当前 Reflect.get 的返回值是否为 Object，如果是则再通过 reactive 方法做代理， 这样就实现了深度观测。

- 监测数组的时候可能触发多次 get/set，那么如何防止触发多次呢？

  我们可以判断 key 是否为当前被代理对象 target 自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行 trigger。

> #### 再说一下 vue2.x 中如何监测数组变化

使用了函数劫持的方式，重写了数组的方法，Vue 将 data 中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样当调用数组 api 时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。

> #### Vue 生命周期

<font color="#f06">beforeCreate</font> 是 new Vue()之后触发的第一个钩子，在当前阶段 data、methods、computed 以及 watch 上的数据和方法都不能被访问。

<font color="#f06">created</font> 在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发 updated 函数。可以做一些初始数据的获取，在当前阶段无法与 Dom 进行交互，如果非要想，可以通过 vm.\$nextTick 来访问 Dom。

<font color="#f06">beforeMount</font> 发生在挂载之前，在这之前 template 模板已导入渲染函数编译。而当前阶段虚拟 Dom 已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发 updated。

<font color="#f06">mounted</font> 在挂载完成后发生，在当前阶段，真实的 Dom 挂载完毕，数据完成双向绑定，可以访问到 Dom 节点，使用\$refs 属性对 Dom 进行操作。

<font color="#f06">beforeUpdate</font> 发生在更新之前，也就是响应式数据发生更新，虚拟 dom 重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。

<font color="#f06">updated</font> 发生在更新完成之后，当前阶段组件 Dom 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

<font color="#f06">beforeDestroy</font> 发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

<font color="#f06">destroyed</font> 发生在实例销毁之后，这个时候只剩下了 dom 空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。

<img src="imgs/vue_01.png">

> #### Vue2.x 组件通信有哪些方式？

1. 父子组件通信

   - 父->子 <font color="#f06">props</font>，子->父 <font color="#f06">$on</font>、<font color="#f06">$emit</font>
   - 获取父子组件实例 <font color="#f06">$parent</font>、<font color="#f06">$children</font>
   - Ref 获取实例的方式调用组件的属性或者方法
   - Provide、inject 官方不推荐使用，但是写组件库时很常用

2. 兄弟组件通信

   - Event Bus 实现跨组件通信 Vue.prototype.\$bus = new Vue
   - Vuex

3. 跨级组件通信

   - Vuex
   - $attrs、$listeners
   - Provide、inject

> #### 你的接口请求一般放在哪个生命周期中？

接口请求一般放在 mounted 中，但需要注意的是服务端渲染时不支持 mounted，需要放到 created 中。

> #### Ajax 请求你一般放在哪个生命周期中

一般建议放在 <font color="#f06">mounted</font> 中。

首先，一个组件的 created 比 mounted 也早调用不了几微秒，性能没啥提高；而且，等到异步渲染开启的时候，created 就可能被中途打断，中断之后渲染又要重做一遍，想一想，在 created 中做 ajax 调用，代码里看到只有调用一次，但是实际上可能调用 N 多次，这明显不合适。相反，若把发 ajax 放在 mounted，因为 mounted 在第二阶段，所以绝对不会多次重复调用，这才是 ajax 合适的位置.

> #### 说一下 Computed 和 Watch

<font color="#f06">Computed</font> 本质是一个具备缓存的 watcher，依赖的属性发生变化就会更新视图。适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。

<font color="#f06">Watch</font> 没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要深度监听对象中的属性时，可以打开 `deep：true` 选项，这样便会对对象中的每一项进行监听。

使用 <font color="#96f">Watch</font> 的深度监听可能会带来性能问题，优化的话可以使用字符串形式监听，如果没有写到组件中，也就是使用 vm.$watch来设置监听的时候，这个vm.$watch 是会返回一个取消观察函数，调用这个函数就可以手动注销监听了。

> #### v-if 和 v-show 的区别

当条件不成立时，v-if 不会渲染 DOM 元素，v-show 操作的是样式(display)，切换当前 DOM 的显示和隐藏。

（从 Vue 源码的角度上来看，v-if 的判断应该是发生在 template 编译成 render function 的过程中）

> #### Vue 的性能优化？

- <font color="#fedcba">编码阶段</font>

  - 尽量减少 data 中的数据，data 中的数据都会增加 getter 和 setter，会收集对应的 watcher
  - v-if 和 v-for 不能连用
  - 如果需要使用 v-for 给每项元素绑定事件时使用事件代理
  - SPA 页面采用 keep-alive 缓存组件
  - 在更多的情况下，使用 v-if 替代 v-show
  - key 保证唯一
  - 使用路由懒加载、异步组件
  - 防抖、节流
  - 第三方模块按需导入
  - 长列表滚动到可视区域动态加载
  - 图片懒加载
  - 将一些常用的方法封装成 library 发布到 npm 上，然后在多个项目中引用
  - 也会将一些公用的组件封装成一个单独的项目然后使用 webpack 打包发布到 npm 上

- <font color="#fedcba">SEO 优化</font>

  - 预渲染
  - 服务端渲染 SSR

- <font color="#fedcba">打包优化</font>

  - 压缩代码
  - Tree Shaking/Scope Hoisting
  - 使用 cdn 加载第三方模块
  - 多线程打包 happypack
  - splitChunks 抽离公共文件
  - sourceMap 优化

- <font color="#fedcba">用户体验</font>

  - 使用 loading 或者骨架屏
  - PWA
  - 封装一个 svg 的组件来使用 iconfont 上的彩色图标，丰富页面
  - 使用缓存(客户端缓存、服务端缓存)优化、服务端开启 gzip 压缩等。

> #### 组件中的 data 为什么是一个函数？

一个组件被复用多次的话，也就会创建多个实例。本质上，`这些实例用的都是同一个构造函数`。如果 data 是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间 data 不冲突，data 必须是一个函数。

> #### Vue 事件绑定原理说一下

原生事件绑定是通过 <font color="#f06">addEventListener</font> 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的<font color="#f06">\$on</font> 实现的。

> #### keep-alive 了解吗

<font color="#96f">keep-alive</font> 可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

主要是有 `include、exclude、max` 三个属性；前两个属性允许 keep-alive 有条件的进行缓存；max 可以定义组件最大的缓存个数，如果超过了这个个数的话，在下一个新实例创建之前，就会将以缓存组件中最久没有被访问到的实例销毁掉。
两个生命周期 activated/deactivated，用来得知当前组件是否处于活跃状态。

> #### Vue-router

- mode
  - `hash`
  - `history`
- 跳转
  - `this.$router.push()`
  - `<router-link to=""></router-link>`
- 占位
  - `<router-view></router-view>`

> #### hash 路由和 history 路由实现原理说一下

- location.hash 的值实际就是 URL 中`#`后面的东西。

- history 实际采用了 HTML5 中提供的 API 来实现，主要有 `history.pushState()`和`history.replaceState()`。

> #### Vuex

- state: 状态中心
- mutations: 更改状态
- actions: 异步更改状态
- getters: 获取状态
- modules: 将 state 分成多个 modules，便于管理

> #### 路由守卫

> #### Vue 原理

> #### 虚拟 DOM

> #### MVC / MVVM
