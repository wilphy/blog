### HTML

#### HTML 语义化

> 根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。  
> 尽可能少的使用无语义的标签 div 和 span；  
> 不要使用纯样式标签，如：b、font、u 等，改用 css 设置。

#### HTML5

> 新增

#### meta标签

```
<!DOCTYPE html>  H5标准声明，使用 HTML5 doctype，不区分大小写
<head lang=”en”> 标准的 lang 属性写法
<meta charset=’utf-8′>    声明文档使用的字符编码
<meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>   优先使用 IE 最新版本和 Chrome
<meta name=”description” content=”不超过150个字符”/>       页面描述
<meta name=”keywords” content=””/>      页面关键词
<meta name=”author” content=”name, email@gmail.com”/>    网页作者
<meta name=”robots” content=”index,follow”/>      搜索引擎抓取
<meta name=”viewport” content=”initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no”> 为移动设备添加 viewport
<meta name=”apple-mobile-web-app-title” content=”标题”> iOS 设备 begin
<meta name=”apple-mobile-web-app-capable” content=”yes”/>  添加到主屏后的标题（iOS 6 新增）
是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏
<meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”>
添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）
<meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
<meta name=”format-detection” content=”telphone=no, email=no”/>  设置苹果工具栏颜色
<meta name=”renderer” content=”webkit”>  启用360浏览器的极速模式(webkit)
<meta http-equiv=”X-UA-Compatible” content=”IE=edge”>     避免IE使用兼容模式
<meta http-equiv=”Cache-Control” content=”no-siteapp” />    不让百度转码
<meta name=”HandheldFriendly” content=”true”>     针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓
<meta name=”MobileOptimized” content=”320″>   微软的老式浏览器
<meta name=”screen-orientation” content=”portrait”>   uc强制竖屏
<meta name=”x5-orientation” content=”portrait”>    QQ强制竖屏
<meta name=”full-screen” content=”yes”>              UC强制全屏
<meta name=”x5-fullscreen” content=”true”>       QQ强制全屏
<meta name=”browsermode” content=”application”>   UC应用模式
<meta name=”x5-page-mode” content=”app”>    QQ应用模式
<meta name=”msapplication-tap-highlight” content=”no”>    windows phone 点击无高光
设置页面不缓存
<meta http-equiv=”pragma” content=”no-cache”>
<meta http-equiv=”cache-control” content=”no-cache”>
<meta http-equiv=”expires” content=”0″>

```

#### Canvas

> 常用 api:

```javascript
fillRect(x, y, width, height); //实心矩形
strokeRect(x, y, width, height); //空心矩形
fillText("Hello world", 200, 200); //实心文字
strokeText("Hello world", 200, 300); //空心文字
```

#### 浏览器内核

---

### CSS

#### 盒模型

> 标准模型：  
> `box-sizing: content-box;` width = content + padding + border  
> IE 模型：  
> `box-sizing: border-box;` width = content

#### 水平垂直居中

#### Flex

#### 选择器

#### BFC

#### 清除浮动

#### 消除图片底部间隙的方法

> 图片块状化 - 无基线对齐：`img { display: block; }`  
> 图片底线对齐：`img { vertical-align: bottom; }`  
> 行高足够小 - 基线位置上移：`.box { line-height: 0; }`

#### 动画 transition / animation

#### 如何触发或避免重绘/重排

#### css reset 和 normalize.css 有什么区别

---

### JS

#### 数据类型

#### 原型与原型链

#### 作用域与闭包

#### 异步与单线程

#### 手写 Promise

#### Promise.all

#### Promise.race

#### async / await

#### 手写函数防抖和节流

> 防抖  
> 触发高频事件后 n 秒内函数只会执行一次，如果 n 秒内高频事件再次被触发，则重新计算时间；  
> 思路：每次触发事件时都取消之前的延时调用方法：  
> 乞丐版：

```javascript
function debounce(fn) {
  let timeout = null; // 创建一个标记用来存放定时器的返回值
  return function () {
    clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
    timeout = setTimeout(() => {
      // 然后又创建一个新的 setTimeout
      // 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
      fn.apply(this, arguments);
    }, 500);
  };
}
function sayHi() {
  console.log("防抖成功");
}

var inp = document.getElementById("inp");
inp.addEventListener("input", debounce(sayHi)); // 防抖
```

> 节流  
> 高频事件触发，但在 n 秒内只会执行一次，所以节流会稀释函数的执行频率。
> 思路：每次触发事件时都判断当前是否有等待执行的延时函数。  
> 乞丐版：

```javascript
function throttle(fn) {
  let canRun = true; // 通过闭包保存一个标记
  return function () {
    if (!canRun) return; // 在函数开头判断标记是否为 true，不为 true 则 return
    canRun = false; // 立即设置为 false
    setTimeout(() => {
      // 将外部传入的函数的执行放在 setTimeout 中
      fn.apply(this, arguments);
      // 最后在 setTimeout 执行完毕后再把标记设置为 true(关键) 表示可以执行下一次循环了
      // 当定时器没有执行的时候标记永远是 false，在开头被 return 掉
      canRun = true;
    }, 500);
  };
}
function sayHi(e) {
  console.log(e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener("resize", throttle(sayHi));
```

#### 手写 Ajax

#### this

#### call / bind / apply

#### 浅拷贝/深拷贝

#### map / filter / reduce / forEach

##### map
> map 的作用是 map 中传入一个函数，该函数会遍历该数组，对每一个元素做变换之后返回新数组。
##### filter
##### reduce
##### forEach

#### 数组去重

#### 正则

> string.trim 去空格

#### let / var / const

### DOM

#### DOM 事件模型

#### 移动端触摸事件

### 网络

#### HTTP 状态码

#### Cookie / LocalStorage / Session

#### CDN

#### GET / POST / PUT

#### 跨域 CORS / JSONP

### Vue

#### Vue 生命周期钩子

#### 组件通信

#### Methods / Computed / Watch

#### Vuex

#### VueRouter

#### 路由守卫

#### Vue 原理

#### 虚拟 DOM

#### MVC / MVVM

### other

#### 性能优化

#### 首屏加载优化

> 降低请求量：合并资源，减少 HTTP 请求数，minify / gzip 压缩，webP，lazyload。
> 加快请求速度：预解析 DNS，减少域名数，并行加载，CDN 分发。
> 增加缓存：HTTP 协议缓存请求，离线缓存 manifest，离线数据缓存 localStorage、PWA。
> 渲染优化：首屏内容最小化，JS/CSS 优化，加载顺序，服务端渲染，pipeline。

#### 骨架屏

#### 排序算法：冒泡，快排，选择，插入

#### XSS / CSRF

#### TypeScript

#### Webpack

#### Echarts.js

#### Flutter

#### SSR

```

```
